# AWS CDK Static Site

This project contains a reusable AWS CDK construct to deploy a static website on AWS. It sets up the necessary infrastructure for serving a static site over HTTPS using Amazon S3 and Amazon CloudFront.

## Architecture

The CDK stack creates the following AWS resources:

*   **Amazon S3:** An S3 bucket is created to store the static website content (e.g., HTML, CSS, JavaScript files). The bucket is configured for public read access.
*   **Amazon CloudFront:** A CloudFront distribution is set up to serve the content from the S3 bucket. It provides HTTPS and caches the content at edge locations for lower latency.
*   **AWS Certificate Manager (ACM):** An SSL/TLS certificate is automatically provisioned from ACM to enable HTTPS for the custom domain. The certificate is validated using DNS.
*   **Amazon Route 53:** An A record is created in your Route 53 hosted zone to point your custom domain to the CloudFront distribution.
*   **AWS S3 Deployment:** The contents of the `site-contents` directory are automatically deployed to the S3 bucket.

## Prerequisites

*   An AWS account.
*   The AWS CDK CLI installed and configured.
*   A registered domain name with a hosted zone in Route 53.

## Setup and Deployment

1.  **Clone the repository:**
    ```bash
    git clone <repository-url>
    cd <repository-directory>
    ```

2.  **Install dependencies:**
    ```bash
    npm install
    ```

3.  **Configure the domain:**

    The stack requires you to specify your domain name and a subdomain. You can do this in two ways:

    *   **Option A: Using `cdk.json`**

        Add the following context variables to your `cdk.json` file:

        ```json
        {
          "context": {
            "domain": "your-domain.com",
            "subdomain": "www"
          }
        }
        ```

    *   **Option B: Using the command line**

        Pass the domain and subdomain as context parameters when running CDK commands:

        ```bash
        cdk deploy -c domain=your-domain.com -c subdomain=www
        ```

4.  **Deploy the stack:**

    ```bash
    cdk deploy
    ```

    The CDK will provision the necessary AWS resources. This process may take some time, especially the first time as it validates the ACM certificate.

## Customizing Site Content

The content of the static site is located in the `site-contents` directory. You can modify the `index.html`, `error.html`, and any other files in this directory to customize your site. After making changes, run `cdk deploy` again to upload the new content.

## Useful CDK Commands

*   `npm run build`: Compiles the TypeScript code.
*   `cdk synth`: Emits the synthesized CloudFormation template.
*   `cdk deploy`: Deploys the stack to your default AWS account/region.
*   `cdk diff`: Compares the deployed stack with the current state.
*   `cdk destroy`: Removes the stack from your AWS account.

## License

This project is licensed under the Apache-2.0 License.
