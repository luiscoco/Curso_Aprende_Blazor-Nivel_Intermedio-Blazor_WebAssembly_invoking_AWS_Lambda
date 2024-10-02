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

After pressing the menu option Develop->CORS we input the 

## 2. 

## 3. 


