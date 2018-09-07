---
title: The coding interview
description: The coding interview
category: blog
---

When you work as software engineer, sooner or later, you will face a coding interview. Sooner if you happen to work in remote.

I think it is important that your employer knows you, and get an idea of what you can and cannot do approximately. If they don't, it is not good for anyone, as you may end up
 in a position where either you feel underproductive, which may lead to seek another job, or maybe you don't deliver, which may cause your employer to seek another engineer to 
fill your position.

Having that said, there is the opposite extreme, being too *enthusiastic* about the coding interview and technical interview in general.

I've been working in this industry for a number of years and i've seen a lot of those. And also a lack of understanding (or lack of being able to communicate to applicants) of the 
qualifications needed for a given position.

Let's start from the *wannabes*. Those are companys that don't have a good HR policy and are driven by marketing moves. They heard it's cool to hire with a process like the one Google does. You end up either with useless and trivial questions like "In what Java package is the String class defined?" (true story) or with useless and very hard questions like "Is it possible to define a convex hull algorithm with complexity O(n)? In what cases? And if not, what is the best achivable complexity? Can you code by hand here in 5 minutes in this blackboard? Ah, and please no pseudocode, something the compiler would accept" (also true story).

Maybe the latter does not sound like a useless question but simply a hard one. This depends on two things: if the company does dedicate to something related with computational geometry or if this is not the first question thrown at you in a general interview. As every software engineer knows, concrete hard problem solving is something that usually needs quietness and access to resources that you don't need to keep safe in your finite memory.

*Wannabe's* companies suffer more the the latter than from the former question. Even Google HR head explain in this [link](https://www.businessinsider.com/how-google-hires-people-2013-6?IR=T) why this is lame. That's the old view of HR as its name implies: resources.

Then, there are the very focused companies. They usually ask for unreasonable amount of time dedicated to the interview. Lately, i've had one of those. I run through a [codility](http://www.codility.com) group of coding exercises. But then, after completed and having been in the 3% that made that far (*Congratulations, 97% of candidates didn't make it to here*), they asked me to spend a whole weekend to run something like a real-live-coding-in-job thing.

The idea was that they would send me a task saturday morning (i wouldn't know nothing of the problem selected beforehand), and i had the weekend to finish it, where they would not only measure completion of the task, and code quality (they didn't tell me to what standards), but also, git use, integration with tools, and things like that. Well, **enough is enough**. Also, citing [Google](http://hrmasia.com/content/hr-summit-2018-how-google-simplified-its-hiring-process), long and unresonable processes are of no good.

Lastly (and luckily), there are the companies that know what they do and what they need. They usually schedule an initial screening to get to know the person behind the name, talk about background, and possibly ask high level technical questions. They then may ask for some coding, but not very long, something reasonable, focused in their domain. Also generally instead of live coding (which is stressful), they would sent the assignment beforehand and set a due date to send back. If there are still doubts, a final interview will do.

I remember a very original way to start the hiring process. Company's name was Adku and at the time, i didn't apply as i didn't consider remote working. They published in their hiring page an algorithmical problem, and the solution of that problem gave you the email address contact to send your CV to. Problem was as follows: *find all possible ways to go from position (0, 0) to position (N - 1, N - 1) in a square matrix for N = 6, given that no diagonal moves are allowed. Send your CV to solution@adku.com*.

[Here](https://github.com/yandroskaos/adku) you can find the solution in plain C.
