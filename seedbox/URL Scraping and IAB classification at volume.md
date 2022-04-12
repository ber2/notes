This evergreen note will document a plan to deliver a IAB classification of URLs and domains at volume.

There are three distinct problems:

1. Training an NLP model capable of classifying IAB URLs.
2. Retrieving the contents of large quantities of URLs, aka __The Scraping Problem__
3. Using the NLP model to classify URL contents.
4. Serving the model in real-time for people to be able to interact with it.

```mermaid
flowchart TD
  subgraph Train
    trainingData[(LabelledData)]
    train(Model fitting)
    registry[(MLFlow Registry)]
    trainingData --> train
    train --> registry
  end
  
  subgraph Scraping
    mediagrid[(Mediagrid Inventory)]
    xandr[(Xandr Inventory - impressions)]
    thirdParty[(Third Party Inventory ST/TT)]
    newUrls(Discovery of new URLs)
    scrapJob[Scraping job]
    docDb[(URL Content)]
    mediagrid --> newUrls
    xandr --> newUrls
    thirdParty --> newUrls
    newUrls --> scrapJob
    scrapJob --> docDb
    docDb -->|cache expiration| newUrls
  end

  subgraph Classification
    classificationJob[Classification Job]
    classificationDb[(URL IAB Classification)]
    classificationJob --> classificationDb
  end

  docDb --> classificationJob

  subgraph Real-time
    dataApp[Data App]
    dataApi[REST API]
  end

  subgraph Batch jobs
    contextualCategorySegments(Contextual Category Segments)
    behaviouralCategorySegments(Behavioural Category Segments)
    reporting[Reporting Use Cases]
  end

  
  classificationDb --> dataApi
  dataApi --> dataApp
  docDb --> dataApi
  registry --> dataApi
  registry --> classificationJob
  classificationDb --> contextualCategorySegments
  classificationDb --> behaviouralCategorySegments
  classificationDb --> reporting
```