---
pagename: LivePerson NLU Engine
redirect_from:
Keywords:
sitesection: Documents
categoryname: "Conversational AI"
documentname: Intent Manager
subfoldername: Natural Language Understanding
permalink: intent-manager-natural-language-understanding-liveperson-nlu-engine.html
indicator: both
---

There are two versions of LivePerson's NLU engine: 
* **LivePerson**: Formerly known as LivePerson NLU v2
* **LivePerson (Legacy)**: Formerly known as LivePerson NLU v1

### LivePerson NLU engine

This is LivePerson's high-performing NLU engine. Key characteristics include:

* A classifier model based on Convolutional Neural Network (CNN) using FastText embeddings.
* Its primary feature is the enabling of a separate, brand-specific model, built and *trained* for each domain.
* A scalable solution that's capable of handling a large volume of requests.
* Provides fast response times and accuracy.
* To perform effectively, expects large sets of data (both intents and training phrases).
* The performance of a model depends on both the quantitative and qualitative nature of the training data. Therefore, we require a minimum of **5 intents** with **15 training phrases per intent** to activate training for a model in the Intent Manager. Anything less adversely affects model performance.
* Requires the model to be [trained](intent-manager-build-domains.html#train-a-liveperson-domain).

{: .important}
As communicated in the release notes, on February 17, 2021 LivePerson enhanced the LivePerson engine to further improve its NLU performance. If you retrained your domain after this date, you’re all set: The domain has picked up the enhancement. If you haven’t done so, please retrain your domain as soon as possible, so it benefits from the change. No additional tuning is required; simply retrain the domain as is.

### Benefits of LivePerson over LivePerson (Legacy)

There are significant benefits to using the LivePerson engine instead of the LivePerson (Legacy) engine for NLU. The LivePerson engine:

* Benefits from continuing investments. Check the roadmap for upcoming features, for example, support for additional languages.
* Performs faster and scales better. The LivePerson engine's average response time is approximately 200 milliseconds per prediction, and performance is good and consistent regardless of the number of intents involved. In contrast, the LivePerson (Legacy) engine's average response time is typically above 1 second, and the response time increases linearly with the number of intents involved. With many intents and many training phrases, a response time of 4-5 seconds is common.
* Provides better accuracy and yields no ambiguity in predictions. A properly trained domain using the LivePerson engine doesn't emit ambiguous results. In contrast, with a domain using LivePerson (Legacy), multiple intents in a domain can have a good match, so ambiguity must be handled in the bot.
* Supports [prebuilt domains](intent-manager-key-terms-concepts.html#prebuilt-domains) to get you up and running quickly, and [Regular Expression entities](intent-manager-key-terms-concepts.html#entities).
* Makes available an advanced [Model Tester](intent-manager-build-test-with-the-model-tester.html) for improving domain quality over time.
* Is widely used with Intent Manager's "Analyze" features. In fact, these features themselves use the LivePerson engine. Therefore, domains using the LivePerson engine can be tuned easily using the data gleaned from Intent Manager. You cannot use the "Analyze"  features with domains using the LivePerson (Legacy) engine.

### Variances in matched intents with LivePerson NLU

When using LivePerson tools ([Model Tester](intent-manager-build-test-with-the-model-tester.html), [Single Utterance Tester](intent-manager-build-test-a-single-utterance.html)) to improve intent classification results, on occasion you might notice a small number of changes in the matched intents for the test set/test phrase after retraining with no additional training samples. There are a number of contributing factors for this observed variance. Some factors are the by-product of the training algorithm, while others can be tackled by changes to the taxonomy of intents or to the training phrases.

#### Randomness in deep learning 
LivePerson's NLU utilizes the latest, deep learning technology to achieve the best intent classification performance. Randomness is baked into the training of deep learning models to facilitate the exploration of optimal solutions. The cost of such explorations is that no two models trained on the same dataset yield perfectly identical results. Our repeated tests have shown that despite the potential variance in the matched intents, the overall performance of our models stays stable. In other words, after retraining with the same dataset, what you might lose in one intent is usually recovered by some improvement in others.

#### Intent overlaps or multiple intents
The variance in matched intents might also indicate that:

* There's a potential intent overlap in the taxonomy, or there's a test sample that contains multiple intents.
* The training samples for different intents lack diversity or distinction.

When working with multiple developers to create an intent domain with a large taxonomy of intents, it is highly likely that duplicate intents will be created over time, or an overlap between intents will develop. For example, “refund - general” and “refund - defective product” overlap, as the former subsumes the latter. 

Similarly, it is also likely that a message such as “I’d like to return my phone and get a refund” might be matched with the “refund” intent or the “return product” intent depending on the training, as we currently only return the top scoring matched intent for each prediction, and both are valid matches for the message. 

A model might also become confused with a fine-grained taxonomy of intents if the training examples are not carefully curated. The intent “refund - defective product” and “refund - no longer needed” might become problematic if the training samples for both intents are limited and if both share similar patterns starting with “I want to get a refund.” 

All these factors directly influence the intent matching after retraining. Therefore, it's important to ensure that:

* The taxonomy of intents is carefully reviewed for overlaps.
* The test samples contain clear, single intents.
* The training samples are diverse within an intent and distinguishable from other intents.

### LivePerson (Legacy) NLU engine

This is LivePerson's legacy NLU engine. 

{: .important}
As of March 5, 2021, you can't create new domains using the LivePerson (Legacy) engine. Brands with existing domains using this engine are encouraged to [migrate to the LivePerson engine](intent-manager-build-domains.html#convert-a-liveperson-legacy-domain-to-liveperson) as soon as possible. LivePerson will deprecate the LivePerson (Legacy) engine at the end of 2021. See above for the many benefits of the LivePerson engine over LivePerson (Legacy).

Key characteristics include:

* A recommender model based on Word Mover's Distance (WMD) algorithms. 
* Considered an "entry level" NLU engine because it's more specific. In other words, for the algorithm to work well, the sample sentences should be close to the expected user input and have only small differences in wording, for example:

    Expected user input: *I want to buy a brown Michael Kors bag*
    <br>Tailored sample sentence (with entities): *I want to buy COLOR PRODUCT_BRAND bag*

    In contrast, the LivePerson engine is more generalized; it can handle a general set of user questions and still perform well. 

* If you have more than 5 intents and more than 15 training phrases per intent, there is a degradation of speed at runtime when processing the user inputs.
* For performance reasons:
    * Supports a maximum of 40 training phrases per intent. If you add more than 40, only the first 40 are used.
    * Supports a maximum of 20 positive learnings per Knowledge Base article. If you add more than 20, only the first 20 are used. There is no limit on the number of negative learnings; however, see the best practices discussed [here](knowledgeai-internal-knowledge-bases-best-practices.html).
* Doesn't require the model to be trained.
