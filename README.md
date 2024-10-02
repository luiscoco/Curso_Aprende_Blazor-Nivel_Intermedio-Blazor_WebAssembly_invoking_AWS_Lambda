# How to invoke, in a Blazor WebAssembly application, an AWS Lambda for creating AWS S3 bucket

**Important Note**:

In this sample we are going to create a Blazor WebAssembly application for inoviking the AWL Lambda created in this github repo:

https://github.com/luiscoco/Curso_Aprende_Blazor-Nivel_Intermedio-AWS_Lambda_Create_S3

But First we are going to create an AWS API GateWay for redirect the Blazor application request to the AWS Lambda

## 1. How to create the AWS API GateWay

We navigate to AWS API GateWay service and create a new **HTTP API** and press the **Build** button

![image](https://github.com/user-attachments/assets/d599ff26-7c73-4f44-b109-144f8b18e1eb)

We press the **Integration** button and then set the Lambda integration type, region and lambda function type

Also we input a **API name** and press **Next** button

![image](https://github.com/user-attachments/assets/06c10bb2-d1d4-4536-8aa6-dcdaa01ddb77)

We configure the routes and press the Next button

![image](https://github.com/user-attachments/assets/77863706-1583-4c4e-af73-a65e0a18f904)

We create a new Stage and press the Next button

![image](https://github.com/user-attachments/assets/7ade4ba8-f276-44fd-a1c6-11b1a1122e26)

Finally, we press the Create button

![image](https://github.com/user-attachments/assets/b38ac881-ffca-4012-943f-7597205a3772)

After creating the API GateWay we have to configure the CORS

![image](https://github.com/user-attachments/assets/b4fd8959-a789-4235-875e-98e34515a59c)

After pressing the menu option Develop->CORS we also press the Configure button

![image](https://github.com/user-attachments/assets/44ab8e50-0a8a-41c5-a031-ac071b4cd5b7)

Now we have to input the allowed values for CORS and press the Save button

![image](https://github.com/user-attachments/assets/2d225195-31a9-4ec3-bc96-ac68235eafb8)

After configuring CORS we have to deploy the AWS API GateWay and select the Produ Stage and press the Deploy button

![image](https://github.com/user-attachments/assets/f9cacd54-e594-4318-8d8e-b60fc80db056)

![image](https://github.com/user-attachments/assets/bb7e8612-029e-4bd2-b6a6-ffe0de94cc6f)

![image](https://github.com/user-attachments/assets/a05f4808-23c5-4089-a749-6f5eb8c0f01f)

We copy the Invoke URL for setting this value in the Blazor WebAssembly application

![image](https://github.com/user-attachments/assets/8aea34d6-c42e-4299-84e8-cbe47d924f89)

## 2. How to create the Blazor Web Assembly application for invoking the AWS API GateWay

We run Visual Studio and we select "**Create a new project**"

![image](https://github.com/user-attachments/assets/a942c8fa-f830-4ca4-bb9b-411241f6eef7)

We select the project template "**Blazor WebAssembly StandAlone App**"

![image](https://github.com/user-attachments/assets/571545da-bec6-47b9-ac28-6844d13589f6)

We input the new project name and location in the hard disk 

![image](https://github.com/user-attachments/assets/3b8a2e24-a699-4b05-b96b-6fd262c2fc3d)

We leave the default values and press create

![image](https://github.com/user-attachments/assets/44a92252-fc48-43a0-90d5-085353b51522)

See the project folders and files structure

![image](https://github.com/user-attachments/assets/81c3464d-16d9-4dea-90af-7073887b0e6a)

## 3. Load the Nuget package

We have to load **AWSSDK.Lambda** package

![image](https://github.com/user-attachments/assets/cc4dfcfb-8f5f-4329-8f97-73d8a5f7cbb0)

## 4. Create a new razor component for invoking the AWS API GateWay

![image](https://github.com/user-attachments/assets/3920f162-25e5-4b9f-bc59-86f42e2567a6)

This is the new component source code:

```csharp
@page "/s3bucket"
@using System.ComponentModel.DataAnnotations
@inject HttpClient Http

<h3>Create S3 Bucket</h3>

<EditForm Model="bucketRequest" OnValidSubmit="OnSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <div class="form-group">
        <label for="bucketName">S3 Bucket Name:</label>
        <InputText id="bucketName" class="form-control" @bind-Value="bucketRequest.BucketName" />
    </div>

    <button type="submit" class="btn btn-primary">Create Bucket</button>
</EditForm>

<div>
    <h4>Response:</h4>
    <p>@responseMessage</p>
</div>

@code {
    private S3BucketRequest bucketRequest = new S3BucketRequest();
    private string responseMessage;

    public class S3BucketRequest
    {
        [Required(ErrorMessage = "Bucket name is required")]
        public string BucketName { get; set; }
    }

    private async Task OnSubmit()
    {
        if (string.IsNullOrEmpty(bucketRequest.BucketName))
        {
            responseMessage = "Bucket name cannot be empty.";
            return;
        }

        try
        {
            // Set up the request body in JSON format
            var lambdaPayload = new { BucketName = bucketRequest.BucketName };
            var jsonPayload = System.Text.Json.JsonSerializer.Serialize(lambdaPayload);

            // API Gateway URL (replace this with your actual API Gateway URL)
            string apiUrl = "https://g37s9y93ve.execute-api.eu-west-3.amazonaws.com/produ/luislambdas3create";

            // Set the request content to JSON
            var content = new StringContent(jsonPayload, System.Text.Encoding.UTF8, "application/json");

            // Call the API Gateway endpoint which triggers Lambda
            var response = await Http.PostAsync(apiUrl, content);

            if (response.IsSuccessStatusCode)
            {
                responseMessage = await response.Content.ReadAsStringAsync();
            }
            else
            {
                responseMessage = "Error invoking Lambda function: " + response.ReasonPhrase;
            }
        }
        catch (Exception ex)
        {
            responseMessage = $"Exception: {ex.Message}";
        }
    }
}
```

