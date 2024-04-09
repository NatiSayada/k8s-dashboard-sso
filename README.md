## Kubernetes Dashboard SSO

This project is a quick and dirty way to add SSO to the Kubernetes Dashboard. It uses the oauth2_proxy to authenticate users and then forwards the request to the Kubernetes Dashboard.

The motivation behind this project was to provide a way to secure the Kubernetes Dashboard and provide a proper way for developers to access the Kubernetes Dashboard.

There was a limited information on how to make it work with Azure AD, so I decided to create this project to help others.

>**NOTE**: This helm chart is not production-ready and you should only use it as a starting point for building your own solution.

### Prerequisites

- A Kubernetes cluster with RBAC enabled
- NGINX Ingress Controller
- Azure AD Application

### Configuration

Create an Azure AD Application and configure it to work with the Kubernetes Dashboard SSO.

#### Azure AD Application

1. Go to the Azure Portal and create a new App Registration.
2. Set the Redirect URI to the Kubernetes Dashboard SSO URL (https://kubernetes-dashboard-sso.example.com/oauth2/callback).
3. Create a new client secret and save it.
4. Copy the Application ID and the Directory ID.
5. Grant the application `User.Read` permissions to `Azure Kubernetes Service AAD Server`, the application ID for `Azure Kubernetes Service AAD Server` will be the same in all Azure AD tenants.

### Installation

To install the Kubernetes Dashboard SSO, you need to follow these steps:

#### Update values.yaml:

1. Update the hostname and the domain you would like to use for the Kubernetes Dashboard SSO.
2. have the `tlsSecretName` set to the secret name of your TLS certificate.

#### Template the Kubernetes Dashboard SSO

Start with templating the Kubernetes Dashboard SSO and see that everything is correct.
```bash
helm template ./ --namespace kubernetes-dashboard > kubernetes-dashboard-sso.yaml
```

#### Install the Kubernetes Dashboard SSO
To install the Kubernetes Dashboard SSO, you need to run the following command:
```bash
helm install kubernetes-dashboard ./ --namespace kubernetes-dashboard
```

