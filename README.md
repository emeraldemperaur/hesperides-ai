# Hesperidesᴬᴵ
## Cloud Native API Orchestration Web Service Application (AWS)
![Changesets](https://img.shields.io/badge/maintained%20with-changesets-176de3?style=flat-square&logo=changesets&logoColor=white) 
[![Release Status](https://github.com/emeraldemperaur/vector-sigma/actions/workflows/release.yml/badge.svg)](https://github.com/emeraldemperaur/vector-sigma/actions)

### Synopsis
<p align="justify">
Cloud native API orchestration web service application for subscribed users to conveniently utilize popular creative productivity AI models or API web services proffered
by xAI, Perplexity, Speechify, Luma Labs (Dream Machine), Midjourney, Imagga and more.
</p>

<p align="justify">
Its raison d'être is to provide imaginative developers with a scalable, production-ready aggregator resource to store and retrieve artifacts & assets (i.e. Text, Image, Audio, Video, Document or Output files) generated from the aforementioned AI services via a unified endpoint(s) for DX convenience and cost consolidation.
</p>

### Core Features
<ol>
<li>
<strong>API Gateway:</strong> Secured endpoint(s) published via AWS API Gateway.</li>
<li>
<strong>User Pools and Authentication:</strong> User identity, authentication and authorization  managed with AWS Cognito & OAuth 2.0.
</li>
<li>
<strong>User Subscriptions:</strong> User subscriptions facilitated by Stripe API in tandem with Stripe Webhooks.
</li>
<li>
<strong>API Services Orchestration:</strong> AI web services orchestration engine designed & engineered utilizing AWS Step Functions, Lambda, DynamoDB and S3.</li>
<li>
<strong>Webhooks and Server-Sent Events (SSE):</strong> Event-driven callback triggers and push updates enabled leveraging DynamoDB Streams, Amazon SQS (Simple Queue Service), ECS (Elastic Container Service), AWS Fargate & EventBridge.</li>
</ol>

### API Documentation & Usage

<ul>
<li><a href="#" target="_blank">Atlas Documentation</a></li>
</ul>

#### Verbs
<table>
  <thead>
    <tr>
      <th align="center">Verb</th>
      <th align="center">AI Service</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>IDEATE</code></td>
      <td>xAI</td>
    </tr>
    <tr>
      <td><code>RESEARCH</code></td>
      <td>Perplexity</td>
    </tr>
    <tr>
      <td><code>VOCALIZE</code></td>
      <td>Speechify</td>
    </tr>
    <tr>
      <td><code>VOCALIZE_TEXT_AS_PODCAST</code></td>
      <td>Speechify</td>
    </tr>
    <tr>
      <td><code>VOCALIZE_DOC_AS_PODCAST</code></td>
      <td>Speechify</td>
    </tr>
    <tr>
      <td><code>RENDER_REEL</code></td>
      <td>Luma Dream Machine</td>
    </tr>
    <tr>
      <td><code>RENDER_IMAGE</code></td>
      <td>Grok Imagine API</td>
    </tr>
    <tr>
      <td><code>TAG_IMAGE</code></td>
      <td>Imagga</td>
    </tr>
    <tr>
      <td><code>TAG_VIDEO</code></td>
      <td>Imagga</td>
    </tr>
  </tbody>
</table>

#### Hesperia Request
<p align="justify">
Hepsperia Request Overview.
</p>

```json
{
  "verb": "IDEATE",
  "context": "From the perspective of a <insert context noun>. Help me develop a...",
  "contextFiles": []
}
```

#### Hesperis Webhooks
<p align="justify">
Hepsperis Webhooks Overview.
</p>

```json
{}
```

#### Asynchronous Polling
<p align="justify">
Hesperides Polling Overview.
</p>

```json
{}
```

#### Server-Sent Events (SSE)
<p align="justify">
Implement Server-Sent Events (SSE) in React using the browser's built-in <a href="https://developer.mozilla.org/en-US/docs/Web/API/EventSource" target="_blank">EventSource API</a> within a React component's <code>useEffect</code> hook</p>

```typescript
// hooks/useSSE.ts
import { useState, useEffect } from 'react';

// Define hook to manage EventSource lifecycle
export const useSSE = (url) => {
  const [data, setData] = useState(null);
  // ... state management for isConnected and error
  
  useEffect(() => {
    // Initialize EventSource, attach listeners, and handle cleanup
  }, [url]);

  return { data, isConnected, error };
};
```

```typescript
// component/NotificationUI.tsx
import { useSSE } from '../hooks/useSSE';

const NotificationSystem = () => {
  // Call the hook with SSE endpoint (e.g. api/sse-notifications)
  const { data, isConnected } = useSSE('http://localhost:6669/api/sse-notifications');

  return (
    // Render connection status and data
  );
};

```

### Integration Screenshots

#### Creative Pipeline Web Application

#### Native Mobile Content Creator App


### System Architecture Design
<ul>
<li>
<p align="justify">
<strong>API Gateway & Router Lambda Layer:</strong> Secured entry point for dynamic Hesperia <em>verb</em> request. Requires valid Cognito JWT to access endpoint to ensure access to only authenticated users with an active subscription and available usage quota. Router Lambda inspects <em>verb</em> attribute in Hesperia request body and validates against a predefined JSON schema for the associated AI web services. All things considered, successful request schema validation initiates the execution of the Ladon AWS Step Function state machine and returns a <code>gardenJobId</code> to the user client.
</p>
</li>
<li>
<p align="justify">
<strong>User Identity, Authentication, Authorization and Subscriptions Layer:</strong> Authentication strata manages creating and verifying user identities and active subscription. User sign-up, login and JSON Web Token generation facilitated by AWS Cognito User Pool. Crucially maps Cognito User IDs to respective Stripe CustomerIDs using DynamoDB table in tandem with Stripe Webhooks for subscription status and usage quota referencing.   
</p>
</li>
<li>
<p align="justify">
<strong>API Orchestration Layer:</strong> Ladon orchestration engine core layer utilizes AWS Step Function to initialize a state machine with a 'choice' state that branches to a dedicated AI worker lambda function predicated on the <em>verb</em> attribute value provided in the Hesperia request object.
</p>
</li>
<li>
<p align="justify">
<strong>Data Persistence & Storage Layer:</strong> AI Worker Lambdas securely store the final output files generated from respective AI web services. Request artifacts (i.e. Audio, Files, Images, Videos, Documents) are downloaded from AI web service provider and uploaded to AWS S3 bucket. Most importantly, the <code>gardenJob</code> status: <code>PENDING</code>, <code>COMPLETED</code>, <code>FAILED</code> and generated presigned S3 <code>articactUrl</code> for every Hesperia request is tracked & updated for referencing using DynamoDB table.
</p>
</li>
<li>
<p align="justify">
<strong>Client Artifacts Retrieval Layer:</strong> Omnichannel client delivery strata provides developer-friendly interface to supports a range of distinct asset retrieval strategies: Asynchronous Polling, Webhooks and Server-Sent Events. 

Webhooks implemented utilizing DynamoDB streams to detect a gardenJob's status change to <code>COMPLETED</code> and triggers a webhook notifier lambda function that sends a HTTP POST request directly to a specified callback server Url. 

Server-Sent Events (SSE) implemented utilizing AWS Fargate and Amazon ECS (Elastic Container Service) in tandem with EventBridge to create a long-lived open HTTP client connection for a specific <code>gardenJob</code> and listen for a <code>COMPLETED</code> event to be emitted by the Ladon Orchestration Engine core layer's AWS Step Fucntion component. 

When a <code>COMPLETED</code> event is emitted for the specific <code>gardenJobId</code>, EventBridge push updates the generated Artificat's S3 presigned URL to the ECS container instance which relays the data directly to the browser through the open stream before closing the connection.
</p>
</li>
</ul>


### Tool Stack

#### AWS Infrastructure
![AWS API Gateway](https://img.shields.io/badge/API%20Gateway-%23FF4F8B.svg?style=for-the-badge&logo=amazonapigateway&logoColor=white)
![AWS Step Functions](https://img.shields.io/badge/Step%20Functions-%23FF4F8B.svg?style=for-the-badge&logo=amazonaws&logoColor=white)
![AWS Lambda](https://img.shields.io/badge/AWS%20Lambda-%23FF9900.svg?style=for-the-badge&logo=awslambda&logoColor=white)
![DynamoDB](https://img.shields.io/badge/DynamoDB-%234053D6.svg?style=for-the-badge&logo=amazondynamodb&logoColor=white)
![Amazon S3](https://img.shields.io/badge/Amazon%20S3-%23569A31.svg?style=for-the-badge&logo=amazons3&logoColor=white)
![Amazon ECS](https://img.shields.io/badge/Amazon%20ECS-%23FF9900.svg?style=for-the-badge&logo=amazonecs&logoColor=white)
![Amazon SQS](https://img.shields.io/badge/Amazon%20SQS-%23FF4F8B.svg?style=for-the-badge&logo=amazonsqs&logoColor=white)
![Amazon EventBridge](https://img.shields.io/badge/EventBridge-%23FF4F8B.svg?style=for-the-badge&logo=amazoneventbridge&logoColor=white)

#### Payment Processing & Subscriptions
![Stripe API](https://img.shields.io/badge/Stripe%20API-%23008CDD.svg?style=for-the-badge&logo=stripe&logoColor=white)
![Stripe Webhooks](https://img.shields.io/badge/Stripe%20Webhooks-%23008CDD.svg?style=for-the-badge&logo=stripe&logoColor=white)

#### AI & Machine Learning APIs
![xAI](https://img.shields.io/badge/xAI-%23000000.svg?style=for-the-badge&logoColor=white)
![Perplexity](https://img.shields.io/badge/Perplexity-%2322B8CD.svg?style=for-the-badge&logo=perplexity&logoColor=white)
![Midjourney](https://img.shields.io/badge/Midjourney-%23FFFFFF.svg?style=for-the-badge&logo=midjourney&logoColor=black)
![Speechify](https://img.shields.io/badge/Speechify-%231E1E1E.svg?style=for-the-badge&logoColor=white)
![Luma Dream Machine](https://img.shields.io/badge/Luma%20Dream%20Machine-%23000000.svg?style=for-the-badge&logoColor=white)
![Imagga](https://img.shields.io/badge/Imagga-%2300B0D8.svg?style=for-the-badge&logoColor=white)


### Resource References
<ul>
<li><a href="https://www.serverless.com/blog/cors-api-gateway-survival-guide" target="_blank">CORS & API Gateway Survival Guide</a></li>
<li><a href="https://auth0.com/docs/get-started" target="_blank">OAuth 2.0</a></li>
<li><a href="https://docs.amplify.aws/" target="_blank">AWS Amplify</a></li>
<li><a href="https://docs.stripe.com/api" target="_blank">Stripe API</a></li>
<li><a href="https://docs.stripe.com/api/webhook_endpoints" target="_blank">Stripe Webhook Endpoints</a></li>
<li><a href="https://docs.x.ai/developers/models" target="_blank">xAI API</a></li>
<li><a href="https://docs.perplexity.ai/docs/getting-started/overview" target="_blank">Perplexity API</a></li>
<li><a href="https://docs.speechify.ai/docs/get-started/overview" target="_blank">Speechify SIMBA API</a></li>
<li><a href="https://docs.lumalabs.ai/docs/api" target="_blank">Luma Labs Dream Machine API</a></li>
<li><a href="https://x.ai/api/imagine" target="_blank">Imagine API</a></li>
<li><a href="https://docs.imagga.com/" target="_blank">Imagga API</a></li>
</ul>
