# Zynga KPIs
Today we're going to break down Zynga's Q4 earning report into Key Performance indicators. 

Zynga Q4 2019 Earnings Report: https://investor.zynga.com/static-files/b61fd24a-7aee-4ebd-8ba8-13e22add64f1

Braze KPI Article: https://www.braze.com/blog/essential-mobile-app-metrics-formulas/

## Approach
For Zynga, revenue comes from users. Users can contribute to revenue in 2 ways: Store purchases and advertisements.
In a freemium game, converting non-paying users to payed users is an active source of income. Advertisement is passive.
Users enter the ecosystem through advertisement campaigns.

1. We track the number of users flowing into and churning out of the app
2. We track the conversion on revenue for users. We monitor payed users, and store interactivity closely.
3. We track the cost of advertisement campaigns vs rewards

## Identifying the Source of Truth through target audience
* It seems like in Q4 December, there was a lot of "fake" traffic. New metrics filter out this traffic.
* Mobile users only. No desktop/web users, because it's insignificant to them, and because of point above.

## Key Performance Indicators from Zynga
**Daily Active Users**
* There is some struggle with identifying individual users.
* This is base metric to track engagement

**Monthly Active Users**
* Number of Unique Users per Month
* An estimate of total game size audience.

**Average Bookings per user (ABPU)**
* Average Daily Bookings / Average DAU
* Avg Daily Bookings = (Items purchased from store + Income from advertisement) /  time period
* Average DAU = DAU averaged / time period
* ABPU = AVG Daily Bookings / Avg DAU = Revenue per user.

## Key Performance Indicators from Braze Article applyed to Zynga
**Retiontion, Churn, New and Total Users**

First we calculate this as a "before and after"
* AU = Active Users. Audience size.
* AU@before is a set of users. AU@After is also a set of users.
* Retained users = Set Intersection of AU@before & AU@after
* Churned Users = Set subtraction of AU@before - AU@after
* Total users = Set union of AU@before | AU@after
* New Users = Set subtraction of AU@after - AU@before
* Retained percent = Retained users / Total users 
* Churned Percent = 1 - Retained percent

**MAU Retention and Churn**
Now we talk about this as a function of time
* Retention = d/dt AU(t) = Change AU / Change Time = (After AU - Before AU) / (After DTTM - Before DTTM)
* Because MAU is audience size. Audience Retention = d/dt MAU(t) = Change MAU / 1 month
* We can track this daily with sliding windows of 30.
* The longer the time period the more stable. The shorted (7 days) the more reactive.

**Stickiness**
* Stickiness is Average DAU for 30 days/MAU
* One component of stickiness is daily sessions / DAU = daily sessions/DAU
* MAU is audience size. DAU is number of users on a given day. We average DAU to remove weekly anomalies.

**Cost Per Aquisition**
* Track for every advertising campaign.
* Cost/# of new users.
* Take Zynga + FB for example. How much are they paying for FB vs number of new users

**Average Daily Bookings**
* Daily bookings

**Lifetime Value**
* Should be greater than CPA.
* Average Value Conversion * Average # of conversion/time * avg customer lifetime = LTV
* Zynga uses ABPU. Time is averaged day. 

**Average Revenue Per User (ARPU) and Average Revenue Per Paying User (ARPPU)**
* This is like current LTV. First 2 terms are Avg bookings. Last term is DAU.
* Total Revenue / Total Users since launch. Not typically useful.
* ABPU which is on average day level is more useful.
* ARPPU is like ABPU, but for paying users only. Average Bookings / AVG MAU.

## Key Performance Indicators for the Store
The store is the central place where most transactions happen. We track this seperately from Advertisement bookings.

**DAPPU and MAPPU**
Tracking Stickiness, Retention, Churn, and Daily/Monthly paying users seperately. These users are usually targetted seperately. In a freemium game, 1% make up for >50% of the revenue.

**Store visites per DAU**
* AVG Times store opened / AVG DAU

**Conversion rate on store**
* Number of store purchases / AVG DAU