---
title: "Data Migration Using AWS Lambda Scheduler"
date: 2024-06-06T20:49:16+07:00
tags: [
  "til",
  "data-migration",
  "today-i-learn",
  "lambda",
]
---

It has been a while since my last post, I did a lof of excuses to do my writing.
I'm not that busy, just enjoying my life, outside digital world. These five months, I have been working on a cool project.
I can not blaberring about the project details here, because I got an NDA agreeement. Fortunately, I have several things I learn and I want to keep as a note here. Long story short, this week I have to deal with data migration, I did some data migrations in the past, but never had a thought to write them down. So, I forgot about the nitty gritty details since then.

At first, The task of migration seems simple when I read the ticket, my client wants all records those have been reached to certain status can not be updated alongside with it relations, put it in another word, they want to create a history logs. I started digging information about what data they needs and how much the data currently in the production needs to be migrated. The database is MySQL, it is straightforward to count the data. 

```
SELECT COUNT(id) FROM orders WHERE status IN ('uncollected', 'cancelled', 'dispensed');
```

the query result only 1615 records. This should be straightforward (that was my thought when looked at the count result). After gathering some more info from the client, I started to write some typescript code on my local. The first version of script logic was querying all the orders data and unroll the body loop . I was processing 5 orders at a time, like the following (not real code, but you got the idea):

```
let orders = await getOrdersWithRelations();

if (!orders.length) {
  log("no orders to be processed");
  continue;
}

for (let i = 0; i < orders.length; i += 5) {
  let order1 = orders[i];

  if (order1) {
  // build order history and save
  }
...
...
...
  let order5 = orders[i+4];
  if (order5) {
  // build order history and save
  }
}
```

Running script migration inside EC2, AWS Batch, or Step Function is out of option, they don't want spawn a new instance, I have to utilize lambda scheduler. There are some limitations we had due to default setting of lambda, the following that I can recall:

1. Default timeout of lambda function is 30s (we can set this to higher number, but that will cost you).
2. Default Max memory in that instance is 1024MB (this is default one).

Then, I ran my code to Dev environment against small amount of data compared to Prod. It took milisecond to ran, then verified the migrated data.
All went good, the day after I started to run my script to Prod, before it hits the loop, script was crashed. I went to cloudwatch to see what was going on. Turn out, my script ran at max available memory, 1024 MiB (my local PC is 32 GiB). 

The second version of the script was, to loop one data at a time, but it was timed out. Then, I tweak my script to move chunk of data at a time, by utilizing offset and limit each loop. The following is roughly approach of my code.

```
let keepRunning = true;
let offset = 0;
let limit = 100;

while (keepRunning) {

  let orders = await getOrdersWithRelations().skip((offset-1) * limit).take(limit);

  if (!orders.length) {
    log("no orders to be processed");
    keepRunning = false;
    return;
  }

  for (let i = 0; i < orders.length; i += 5) {
    let order1 = orders[i];

    if (order1) {
    // build order history and save
    }
  ...
  ...
  ...
    let order5 = orders[i+4];
    if (order5) {
    // build order history and save
    }
  }
  offset += 1;
}
```

It took 56 seconds to complete the process, this like forever for modern computer, I once saw a challenge about 1 Billion rows and they did it under 2 seconds (but I'm not sure about the hardware specs). This was a decent number for me back then, the most important things is get the shit done to have the business keep running. I can do optimization another time outside this task (that was my thought).

Several lessons I got from this simple migration task are:

1. Gather about hardware specifications target of our program will be running on.
2. Try to run the script with more data or at least good sample of data before it went to Prod.
3. Migration should be done in Chunk of data, in order to guarantee the integrity(no redundancy and corruption) and correctness.
4. The script should be fault tolerance and run as efficient as possible and can be resumed.

***PS: Turn out the unrolling loops not necessary makes things faster in high level language like JS, some smart engineers that work on V8 (node engine have done that under the hood).***
