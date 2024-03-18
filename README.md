# MongoDB-VertexAI-Qwiklab

### Pre-requisites
1. MongoDB Atlas access
2. Access to Google Cloud Project to deploy and create a Compute Engine instance

### Create Atlas Vector Search Index
1. Navigate to the Database Deployments page for your project.
2. Click on Create Database. Name your Database vertexaiApp and your collection chat-vec.
3. Click Atlas Search from the Services menu in the navigation bar.
4. Click Create Search Index and select JSON Editor under Atlas Vector Search. Then, click Next.
5. In the Database and Collection section, find the database vertexaiApp, and select the chat-vec collection.
6. Replace the default definition with the following index definition and then click Next. Click on Create Search index on the review page.
```
{
    "fields": [
        {
            "type":"vector",
            "path":"vec",
            "numDimensions":768,
            "similarity": "cosine"
        }
    ]
}
```

Inspired by [this MongoDB blog](https://www.mongodb.com/developer/products/atlas/build-smart-applications-atlas-vector-search-google-vertex-ai/)
