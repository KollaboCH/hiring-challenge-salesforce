# Kollabo Salesforce Challenge. 

## Intro
In the following you will find 2 short tasks to show us your coding skills and your motivation for the job. 🚀

### Challenge 1
For this challenge, you need to write all the code from scratch!

Your mission, should you choose to accept it… 👨‍💻

Write a trigger that will prevent each user from creating more than 99 cases a month!
The maximum number of cases per user should be configurable without code. Each user will have the same maximum.
Users should see the following error message if they break their max:

> Too many cases created this month for user \<\<Name\>\> (\<\<User ID\>\>): \<\<Maximum\>\>
> 
Don't forget your test class!

![alt text](https://img.kollabo.ch/github/Coding%20Challenge%201.png)

### Challenge 2
Can you find everything wrong with this trigger?! 👀
```
// Automatically create a Renewal Opp for closed won deals
trigger CreateRenewal on Opportunity (before update) {

  // Create a Map to store all renewal opps for bulk inserting
  Map<Id, Opportunity> renewals = new Map<Id, Opportunity>();

  for (Opportunity opp : Trigger.new) {
    // Only create renewal opps for closed won deals
    if (opp.StageName.contains('Closed')) {
       Opportunity renewal = new Opportunity();
       renewal.AccountId   = 'opp.AccountId';
       renewal.Name        = opp.Name + 'Renewal';
       renewal.CloseDate   = opp.CloseDate + 365; // Add a year
       renewal.StageName   = 'Open';
       renewal.RecordType  = 'Renewal';
       renewal.OwnerId     = opp.OwnerId;
       renewals.put(renewal.Id, renewal);
    }
  }

  // Bulk insert all renewals to avoid Governor Limits
  insert renewals;
}
```
 
