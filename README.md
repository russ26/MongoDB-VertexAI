# MongoDB-VertexAI
In this tutorial, we will see how to get started with MongoDB Atlas and Vertex AI.

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

### Create a Google Cloud Compute instance
We will create a Google Cloud virtual machine instance to run and deploy the application. The Google Cloud VM can have all the default configurations. To begin, log into your Google Cloud Console and perform the following steps:
 
* In the Google Cloud console, click on Navigation menu > Compute Engine.
* Create a new VM instance with the below configurations:
   * Name: vertexai-chatapp
   * Region: region near your physical location
   * Machine configurations:
       * Machine type: High Memory, n1-standard-1
* Boot disk: Click on CHANGE
    * Increase the size to 100 GB.
    * Leave the other options to default (Debian).
* Access: Select Allow full access to all Cloud APIs.
* Firewall: Select all.
* Advanced options:
    * Networking: Expand the default network interface.
        * For External IP range: Expand the section and click on RESERVE STATIC EXTERNAL IP ADDRESS. This will help users to access the deployed application from the internet.
        * Name your IP and click on Done.
* Click on CREATE and the VM will be created in about two to three minutes.
* Ensure port 8501 is added to default-allow-http in Network Security > Firewall policies
* In the Atlas console, add the external IP of the VM to your Atlas project's IP Access List

### Deploy the application
The repository contains a script to create and deploy a Streamlit application to transform and store PDFs in MongoDB Atlas, then search them lightning-fast with Atlas Vector Search. The app.py script in the repository uses Python and LangChain to leverage MongoDB Atlas as our data source and Google Vertex AI for generating embeddings.

We start by setting up connections and then utilize LangChain’s ChatVertexAI and Google's Vertex AI embeddings to transform the PDF being loaded into searchable vectors. Finally, we constructed the Streamlit app structure, enabling users to input queries and view the top retrieved documents based on vector similarity.

Once the VM instance is created, SSH into the VM instance and run the following commands to install the required dependencies: 
```
sudo apt-get update
sudo apt-get install git
sudo apt install python3.11-venv
git clone https://github.com/russ26/MongoDB-VertexAI.git
cd MongoDB-VertexAI
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
```

Once the requirements are installed, you can edit the config file by using the command below to add your Atlas cluster connection string: 
```
nano config.py
```

Once saved, you can run the application using the below command. Open the application using the public IP of your VM and the port mentioned in the command output:
```
streamlit run app.py
```

### Summary
The application uses the PdfReader library to read the PDF file from the user and chunks the data into batches. Each chunk is converted into vectors using LangChain’s VertexAIEmbeddings libraries. The uploaded PDF is written into MongoDB Atlas in chunks, along with the vectors. The data can be viewed in vertexaiApp.chat-vec namespace documents.

To perform a search on the data, navigate to the Q&A tab on your application. Type in your query and press enter. The application will perform a vector search on MongoDB documents using the index created earlier. The application uses the chat-bison model to converse with the user.

Inspired by [this MongoDB blog](https://www.mongodb.com/developer/products/atlas/build-smart-applications-atlas-vector-search-google-vertex-ai/)
