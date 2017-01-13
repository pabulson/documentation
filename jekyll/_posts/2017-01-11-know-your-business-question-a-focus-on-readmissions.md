---
layout: post
title:  "Know your business question: A focus on readmissions"
date:   2017-01-11 21:00:00 -0700
tags: overview
author: Taylor Larsen
categories: blog
excerpt: This post describes the importance of understanding the business questions, use cases, and data when creating a readmission risk model
---

As time goes on, we will not only discuss healthcare machine learning (ML) and health in the US at a high level, but also specific ways ML might help drive outcomes improvements. Many health systems are working on reducing their readmission rate—which is often considered a measure of quality of care and can be tied to penalties. For  hospital systems progressing toward ML for readmissions—or any measure—the first step is to identify your most important business questions; the next step is creating a suitable dataset to create the model. There are often several points where business logic dictates decisions related to the dataset and whether one or multiple models are needed to help with a specific process. That’s certainly the case when creating readmission risk models. 

Readmission risk models improve patient quality of life and decrease mortality by providing extra care or surveillance to high-risk patients. Implementing a readmission risk model could serve two different purposes: 

1.  Identifying high-risk *observational* and *inpatient* patients as soon after their admission as possible to answer this question: *Which in-hospital patients are most at risk for readmission?* By answering this question, doctors, nurses, and in-hospital staff can intervene to try to lower a patient’s readmission risk.

2.  Identifying high-risk *discharged* patients as soon after their discharge as possible to answer this question: *Which discharged patients are most at risk of readmission?* By answering this question, hospital support staff and transitional services can intervene to try to lower a patient’s readmission risk. 

Now, you might think, “Those are the same use case: they both predict readmissions, even sometimes for the same patients.” But remember, an ML model is only valuable if it provides actionable insight. Nurses are in the optimal position to help when the patient is in the hospital. Furthermore, as we [discussed at length](http://healthcare.ai/blog/2017/01/06/data-leakage-in-healthcare-machine-learning/) last week, the data sources for these two use cases are very different. The type and amount of information available at discharge is different from what is available at admission. The best ML models are built custom to a specific dataset and use case. One risk model is not sufficient to answer both questions, and accuracy would almost certainly be compromised if they were naively combined. Though similar, these questions need models that relate to different types of patients and are targeted toward different interventions. Under the hood of the models, we need to use different datasets (to avoid [data leakage](http://healthcare.ai/blog/2017/01/06/data-leakage-in-healthcare-machine-learning/))—and maybe even different algorithms. The best ML model will always be built to answer a specific question, tailored to specific data, and targeted toward the most effective intervention. 

OK, so we know we need two models for readmission risk modeling. But to complicate things even more, we must keep in mind that the layman definition of readmission and the [CMS definition of readmission](https://www.cms.gov/Medicare/Medicare-Fee-for-Service-Payment/PhysicianFeedbackProgram/Downloads/2014-ACR-MIF.pdf) are also subtly different. We must base our outcome variable on a definition very similar to that of CMS because it is consistent with the way many hospital systems track and report outcomes, and the CMS definition of readmissions is ultimately the measure hospital systems are trying to affect. Knowing that definition is critical to building a model to address it. 

Per the CMS definition, patients in the hospital can be *emergency department*, *observational*, or *inpatient*. To be considered an unplanned readmission patients must initially be discharged from the inpatient setting. A patient that is discharged from the emergency department or observation setting cannot be readmitted (0% probability) because they did not meet the inpatient requirement pertaining to the inpatient index admission. Similarly, even after a patient is admitted to the inpatient setting, we still do not yet know their what their discharge disposition or discharge diagnosis will be. If the patient leaves against medical advice or is assigned a cancer-related discharge, for instance, they meet a different set of exclusion criteria and cannot be readmitted (again, 0% probability). While the specific criteria behind the definition of a readmission makes practical sense, it creates a couple of challenges to training, testing, and deploying a readmission risk model that is to be leveraged while patients are still in the hospital. 
    
At the end of the day, we need to develop the model using the same data that we want it to perform well on in production. For the *in-hospital* use case, *observational* and *emergency department* patients that would ultimately be excluded at discharge must also be excluded from model development. These patients may skew the model toward predicting 0% readmission probability since they are guaranteed to be 0% risk as defined by CMS. This puts us in a tight spot. When [evaluating the model](http://healthcare.ai/blog/2016/12/15/model-evaluation-using-roc-curves/), it may appear to have higher accuracy by skewing toward low probabilities because it was improperly trained on data that should have been excluded. For the post-discharge use case, the discharge type is available and the model can match the CMS definition more closely. This will likely lead to increased accuracy overall, as more use-case specific data is available. 

From our experience, understanding the definition of the readmission outcome variable, the specific use case, and the timing/target is crucial. There is clearly a trade-off between timeliness and accuracy, and to have the greatest impact on patient outcomes, it is important to develop readmission risk models based on data that reflects the use case in production. Again, the best ML model should answer a specific question, using specific data, with an actionable result. Keep these things in mind and your models will improve. 

Thanks for reading and please [reach out](http://healthcare.ai/contact) with any questions or comments!