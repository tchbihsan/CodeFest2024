# Challenge 3:

[&lt; Previous Challenge](./Challenge-02.md) - **[Home](../README.md)** - [Next Challenge &gt;](./Challenge-04.md)

### Pre-requisites
Challenge 1 and Challenge 2 completed successfully

### Introduction

Remember the Lambda module that we created in the last challenge? In this module, we will use [Amazon API Gateway](https://aws.amazon.com/api-gateway/?e=gs2020&p=build-a-web-app-three) to create a RESTful API that will allow us to make calls to this Lambda function from a web client (typically refers to a user's web browser). API Gateway will act as a middle layer between the HTML client we created in Challenge 1 and the serverless backend we created in Challenge 2.

Then, we will update the static website we created using s3 in Challenge 1 to invoke the REST API we created in this challenge. This will add the ability to display text based on what you input.

In this Challenge, we'll implement the following architectural diagram:

![image](https://github.com/ExxonMobil/AWSCodefest-Challenge3/assets/77283967/78a6555f-010f-4f56-a396-3461ca134968)

Description:

1. Create an API Gateway:
   - Create a new API using API Gateway.
   - Define the HTTP methods on your API.
   - Enable cross-origin resource sharing (CORS) on an API so you can consume resources from a different origin (domain).
   
3. Integrate API Gateway with AWS Lambda function
   - Trigger a Lambda function from an API.
   - Test an API created with API Gateway from the AWS Management Console.
   
5. Integrate Static WebSite with API Gateway
   - Call an API Gateway API from an HTML page.
   - Upload a new version of a web app to the s3 bucket.

7. Test the web app functionality.

### End Product

### Key Concept

#### 1. Static Website
You can use Amazon S3 to host a static website. On a static website, individual webpages include static content. They might also contain client-side scripts. By contrast, a dynamic website relies on server-side processing, including server-side scripts, such as PHP, JSP, or ASP.NET. Amazon S3 does not support server-side scripting, but AWS has other resources for hosting dynamic websites. Amazon S3 website endpoints also do not support HTTPS. If you want to use HTTPS, you can use Amazon CloudFront to serve a static website hosted on Amazon S3. For more information, see [How do I use CloudFront to serve HTTPS requests for my Amazon S3 bucket?](https://repost.aws/knowledge-center/cloudfront-https-requests-s3) To use HTTPS with a custom domain, see [Configuring a static website using a custom domain registered with Route 53](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html).

You can also use the AWS Amplify Console to host a single-page web app. The AWS Amplify Console supports single-page apps built with single-page app frameworks (for example, React JS, Vue JS, Angular JS, and Nuxt) and static site generators (for example, Gatsby JS, React-static, Jekyll, and Hugo). For more information, see [Getting Started in the AWS Amplify Console User Guide](https://docs.aws.amazon.com/amplify/latest/userguide/getting-started.html).

#### 2. RESTful API 
REST stands for "Representational State Transfer" and is an architectural pattern for creating web services. API stands for "application programming interface." Thus, a RESTful API is one that implements the REST architectural pattern.

#### 3. HTTP request methods
HTTP methods are designed to enable communications between clients and servers. Methods, like GET or PUT defined by the HTTP protocol, are used to indicate what action to take on a resource.

#### 4. CORS
The CORS browser security feature uses HTTP headers to tell a browser to allow a given web application running at one origin (domain) to access selected resources from a server at a different origin.

#### 5. Edge optimized
A resource that uses AWS global infrastructure to better serve geographically diverse clients.

### Implementation

#### 1. CREATE A NEW REST API

1. Make sure the region selected is Ohio / us-east-2.
2. From the search bar in AWS console, type API Gateway.
3. From the left navigation pane under APIs, click Create API. Under Choose an API type, find REST API and click Build.

    ![image](https://github.com/ExxonMobil/AWSCodefest-Challenge3/assets/77283967/e90ba9c2-c476-4c67-9cd5-9b75e3fae586)
   
    ![image](https://github.com/ExxonMobil/AWSCodefest-Challenge3/assets/77283967/7a0b3c8e-85b4-4117-a2c9-e5f824b2a2f8)

4. Under Create new API, enter the following details:
   - API Details: New API.
   - API Name: APItoCallLambda.
   - API Endpoint Type: Edge optimized.

(Note: Edge-optimized endpoints are best for geographically distributed clients. This makes them a good choice for public services being accessed from the internet. Regional endpoints are typically used for APIs that are accessed primarily from within the same AWS Region.) Choose the blue Create API button. Your settings should look like the accompanying screenshot.

   ![image](https://github.com/ExxonMobil/AWSCodefest-Challenge3/assets/77283967/0f1a9c75-23e1-4fce-bc91-79304c0006e0)

5. Verify the API has been successfully created.

   ![image](https://github.com/ExxonMobil/AWSCodefest-Challenge3/assets/77283967/0b23e839-6001-430b-a271-7309185a3bca)


#### 2. CREATE A NEW RESOURCE METHOD

1. In the left navigation pane, select Resources under APItoCallLambda.
2. Ensure the "/" resource is selected.
3. On the bottom right of the screen, you will find a box called Methods. Select Create Method.

   ![image](https://github.com/ExxonMobil/AWSCodefest-Challenge3/assets/77283967/a4022afb-9dd3-4aaa-adb4-01bdb391cf8a)

4. Enter the following in the Method Details:
   - Method type: POST
   - Integration type: Lambda Function
   - Lambda function: Select the Lambda Region you used when making the function (or else you will see a warning box reading "You do not have any Lambda Functions in...") and then Lambda function you created in Challenge 2. Leave the rest empty. Click 'Create Method' orange button to save.
  
![image](https://github.com/ExxonMobil/AWSCodefest-Challenge3/assets/77283967/23c8996e-9ab3-4d1a-a3e3-c623b73b5e32)

#### 3. DEPLOY API

1. From the left navigation pane, make sure Resources under your API called APItoCallLambda is selected.
2. Under Resources, make sure POST is selected.
3. Click Deploy API.
4. In the Deploy API, enter the following details:
   - Stage: Select [*New Stage*].
   - Stage name: dev
5. Choose Deploy.

   ![image](https://github.com/ExxonMobil/AWSCodefest-Challenge3/assets/77283967/cb5454e9-53a3-4dd9-b955-9db6a89d426f)

6. In the left navigation pane, select Stages and ensure dev is selected. In the Stage details, copy the Invoke URL. This is the URL you will use for testing.

   ![image](https://github.com/ExxonMobil/AWSCodefest-Challenge3/assets/77283967/a8106a53-5a22-4739-9b91-1578d5dd241c)
   
#### 4. VALIDATE API

1. In the left navigation pane, select Resources.
2. Select the method POST.
3. Click Test tab.

    ![image](https://github.com/ExxonMobil/AWSCodefest-Challenge3/assets/77283967/0d164d7b-e752-466e-b646-00a21532923d)

4. In the Test Method, enter the following information:
   - Headers:
     ```JSON
     Content-Type: application/json
     ```
   - Request Body:
     ```JSON
     {
        "customername": "Rose",
        "productname": "Yoghurt",
        "quantity": 1
     }
     ```
5. Click Test. Make sure the result you are seeing is as per what you saw when you run Lambda in the previous challenge
Great! We have built and tested an API that calls our Lambda function.

#### 5. UPDATE WEB APP

1. Open the index.html file you created in module one.
2. Replace the existing code with the following:

```HTML

<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>PLACE YOUR ORDER HERE!</title>
    <!-- Styling for the client UI -->
    <style>
    h1 {
        color: #FFFFFF;
        font-family: system-ui;
        margin-left: 20px;
    }
    body {
        background-color: #222629;
    }
    label {
        color: #86C232;
        font-family: system-ui;
        font-size: 20px;
        margin-left: 20px;
        margin-top: 20px;
        display: block;
    }
    button {
        background-color: #86C232;
        border-color: #86C232;
        color: #FFFFFF;
        font-family: system-ui;
        font-size: 20px;
        font-weight: bold;
        margin-left: 20px;
        margin-top: 20px;
        width: 140px;
    }
    select, input[type="text"] {
        color: #222629;
        font-family: system-ui;
        font-size: 20px;
        margin-left: 20px;
        margin-top: 10px;
        width: 200px;
    }
    </style>
    <script>
        // callAPI function that takes the base and exponent numbers as parameters
        var callAPI = (customerName, product, quantity) => {
            // instantiate a headers object
            var myHeaders = new Headers();
            // add content type header to object
            myHeaders.append("Content-Type", "application/json");
            // using built-in JSON utility package turn object to string and store in a variable
            var raw = JSON.stringify({ "customername": customerName, "productname": product, "quantity": quantity});
            // create a JSON object with parameters for API call and store in a variable
            var requestOptions = {
                method: 'POST',
                headers: myHeaders,
                body: raw,
                redirect: 'follow'
            };
            // make API call with parameters and use promises to get response
            fetch("<< REPLACE WITH YOUR API URL >>", requestOptions)
                .then(response => {
                    console.log('Response status:', response.status); // Log the response status
                    return response.json(); // Parse response as JSON
                })
                .then(result => {
                    console.log('Response body:', result); // Log the response body
                    alert(JSON.stringify(result)); // Display the entire response as a string
                })
                .catch(error => console.error('Error:', error)); // Log any errors
        }
    </script>
</head>
<body>
    <h1>PLACE YOUR ORDER HERE!</h1>
    <form>
        <label>Customer Name:</label>
        <input type="text" id="customerName">
        <label>Product:</label>
        <select id="product">
            <option value="book">Book</option>
            <option value="pencil">Pencil</option>
            <option value="eraser">Eraser</option>
        </select>
        <label>Quantity:</label>
        <input type="text" id="quantity">
        <!-- set button onClick method to call function we defined passing input values as parameters -->
        <button type="button" onclick="callAPI(document.getElementById('customerName').value,document.getElementById('product').value, document.getElementById('quantity').value)">Place Order</button>
    </form>
</body>
</html>

```
4. Make sure you add your API Invoke URL on Line XXX. Note: If you do not have your API's URL, you can get it from the API Gateway console by selecting your API and choosing stages.
5. Save the file.
6. Go to your s3 bucket.
7. Replace the index.html file with the updated code.
8. When the file is uploaded, a deployment process will automatically begin. Open the URL. Your Web Application should look like the following:

![image](https://github.com/ExxonMobil/AWSCodeFest-2024/assets/77283967/c76df027-4510-4ee8-8b47-af7423b31b4a)


#### 6. TEST UPDATED WEB APP

1. Choose the URL under Domain.
2. Your updated web app should load in your browser.
3. Fill in your name, select product name and select product quantity. Hit 'Place Order'
4. You should see a message stating whether your request is successful or failed.
`Full answer below. To remove some parts for participants to figure out themselves`
6. Go to your DynamoDB. Ensure new entry is created in the DynamoDB table.

### SUCCESS CRITERIA:
- [] You have a static web app deployed by s3 working that can call a Lambda function via API Gateway.
- [] Data is updated in DynamoDB.
