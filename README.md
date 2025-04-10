# OCP Monitoring and Alerting with Grafana

This repository contains configuration files for setting up monitoring and alerting on OpenShift using Grafana. The setup includes enabling user workload monitoring, deploying the Grafana Operator, configuring Grafana instances, data sources, and dashboards, and exposing Grafana via a route.

## Table of Contents

- [Overview](#overview)
- [Files](#files)
- [Prerequisites](#prerequisites)
- [Setup](#setup)
  - [Step 1: Enable User Workload Monitoring](#step-1-enable-user-workload-monitoring)
  - [Step 2: Deploy Grafana Operator](#step-2-deploy-grafana-operator)
  - [Step 3: Configure Grafana Instance](#step-3-configure-grafana-instance)
  - [Step 4: Add Data Source](#step-4-add-data-source)
  - [Step 5: Add Dashboards](#step-5-add-dashboards)
  - [Step 6: Expose Grafana](#step-6-expose-grafana)
- [License](#license)

## Overview

This setup enables monitoring and alerting for OpenShift workloads using Grafana. It includes:

1. Enabling user workload monitoring.
2. Deploying the Grafana Operator.
3. Configuring a Grafana instance with Prometheus as the data source.
4. Adding custom dashboards for monitoring OpenShift projects and GitOps (ArgoCD).
5. Exposing Grafana via a route for external access.

## Files

- `user-workload-monitoring-config.yaml`: Enables user workload monitoring in OpenShift.
- `Rolebinding.yaml`: Configures RBAC for Grafana service accounts to access cluster monitoring data.
- `grafana-subscription.yaml`: Deploys the Grafana Operator.
- `grafana-instance.yaml`: Configures a Grafana instance with ingress and resource limits.
- `grafana-datasource.yaml`: Configures Prometheus as the data source for Grafana.
- `grafana-dashboard.yaml`: Adds custom dashboards to Grafana.
- `route.yaml`: Exposes Grafana via an OpenShift route.

## Prerequisites

Before setting up monitoring and alerting, ensure the following:

1. You have access to an OpenShift cluster.
2. The `oc` CLI is installed and configured to interact with your OpenShift cluster.
3. You have cluster admin privileges to apply the configurations.

## Setup

### Step 1: Enable User Workload Monitoring

Apply the `user-workload-monitoring-config.yaml` file to enable user workload monitoring:

```bash
oc apply -f user-workload-monitoring-config.yaml
```

### Step 2: Deploy Grafana Operator
Deploy the Grafana Operator by applying the grafana-subscription.yaml file:
```bash
oc apply -f grafana-subscription.yaml
```

### Step 3: Configure Grafana Instance
1. Apply the RBAC configuration for Grafana service accounts:
```bash
oc apply -f Rolebinding.yaml
```

2. Deploy the Grafana instance:
```bash
oc apply -f grafana-instance.yaml
```

### Step 4: Add Data Source 
1. Retrieve the bearer token for the Grafana service account:
```bash
oc get secret -n openshift-user-workload-monitoring grafana-sa -o jsonpath='{.data.token}' | base64 --decode
```

2. Replace <PASTE_BEARER_TOKEN_HERE> in grafana-datasource.yaml with the retrieved token.
```bash
oc apply -f grafana-datasource.yaml
```

3. Apply the data source configuration:
```bash
oc apply -f grafana-datasource.yaml
```

### Step 5: Add Dashboards
Add custom dashboards to Grafana by applying the grafana-dashboard.yaml file:
```bash
oc apply -f grafana-dashboard.yaml
```

### Step 6: Expose Grafana
Expose Grafana via a route by applying the route.yaml file:
```bash
oc apply -f route.yaml
```

Retrieve the route URL:
```bash
oc get route grafana -n openshift-user-workload-monitoring -o jsonpath='{.spec.host}'
```

Access Grafana in your browser using the retrieved URL.

License
This project is licensed under the MIT License. See the LICENSE file for more details.
