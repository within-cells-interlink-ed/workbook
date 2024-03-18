# GCP API

### What is API ?

API stands for Application Programming Interface. It is a set of protocols, standards, and tools that allows different software applications to communicate with each other.

APIs enable software applications to interact with each other by sending and receiving data in a structured format. APIs can be used to access data, services, and functionality provided by other applications or web services.

### What is GCP API?

GCP API stands for Google Cloud Platform API. It is a collection of REST APIs that provide programmatic access to GCP services, allowing you to manage your GCP resources programmatically.

These APIs enable you to interact with various GCP services such as Compute Engine, Kubernetes Engine, Cloud Storage, Cloud SQL, and many others.

So , Now let's check about GCP API misconfiguration below:

### 1 ) Check for API Key API Restrictions

In order to follow cloud security best practices and reduce the attack surface, Google Cloud API keys should be restricted to call only those APIs required by your application.

To determine if your Google Cloud API key usage is restricted to specific APIs only, perform the following operations:

1. Run _**projects list**_ command  with custom query filters to list the ID of each project available in your Google Cloud account:

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project IDs:

```
PROJECT_ID
  cc-project5-112233
  cc-internal-111222
```

3. &#x20;Now run _**services api-keys list**_ command which  Lists the API keys of a given project.

```
gcloud alpha services api-keys list  --project=cc-project5-112233  --format="table(uid)"
```

4. The command output should return the IDs of the active API keys:

```
UID:
  abcd1234-abcd-1234-abcd-1234abcd1234
  1234abcd-1234-abcd-1234-abcd1234abcd
```

5. Run _**services api-keys describe**_ command using the ID of the API key that you want to examine as the identifier parameter and custom query filters to list the names of the Google Cloud APIs that can use the selected API key:

```
gcloud alpha services api-keys describe abcd1234-abcd-1234-abcd-1234abcd1234 --format="json(restrictions)"
```

6. Based on the _**services api-keys describe**_ command output, you can determine whether or not the use of the selected API key is restricted to specific APIs only:

* If the command output returns **null**, there is no API restriction control enabled, therefore the selected API key can call any supported Google Cloud Platform (GCP) API:

```
null
```

* If the command output returns one or more APIs, check the **"apiTargets"** array for the list of GCP APIs that can use the selected API key. If the **"apiTargets"** array contains **"cloudapis.googleapis.com"**, as shown in the example below, the selected API key can call any GCP API because the **"cloudapis.googleapis.com"** option represents the API collection of all the cloud services/APIs offered by Google Cloud Platform:

```
{
  "restrictions": {
    "apiTargets": [
      {
        "service": "cloudapis.googleapis.com"
      }
    ]
  }
}
```

### 2 )Check for API Key Application Restrictions

In order to follow cloud security best practices and reduce the attack surface, Google Cloud API keys should be restricted only to trusted hosts, HTTP referrers, and Android/iOS mobile applications.

To determine if there are any unrestricted API keys within your Google Cloud account, perform the following actions:

1. Run _**projects list**_ command  with custom query filters to list the ID of each project available in your Google Cloud account:

```
gcloud projects list  --format="table(projectId)"
```

2. The command output should return the requested GCP project IDs:

```
PROJECT_ID
  cc-project5-112233
  cc-internal-111222
```

3. &#x20;Now run _**services api-keys list**_ command which  Lists the API keys of a given project.

```
gcloud alpha services api-keys list --project=cc-project5-112233 --format="table(uid)"
```

4. The command output should return the IDs of the active API keys:

```
UID:
  abcd1234-abcd-1234-abcd-1234abcd1234
  1234abcd-1234-abcd-1234-abcd1234abcd
```

5. Run _**services api-keys describe**_ command  using the ID of the API key that you want to examine as the identifier parameter and custom query filters to describe the API key application restrictions configured for the selected key:

```
gcloud alpha services api-keys describe abcd1234-abcd-1234-abcd-1234abcd1234  --format="json(restrictions)"
```

6. Based on the **services api-keys describe** command output, you can determine if the use of the selected API key is unrestricted:

* If the command output returns **null**, there is no restriction control enabled to specify which websites, IP addresses, or mobile applications can use the key, therefore the selected API key usage is unrestricted:

```
null
```

* If the command output returns one or more HTTP referrers for API key application restrictions, as shown in the example above, check the **"allowedReferrers"** array for the list of domains that can use the selected API key.&#x20;

&#x20;    If the referrer is set to a wildcard, i.e. **\*** or **\*.\[TLD]** or **\*.\[TLD]/\***, where \[TLD] represents the top-    level domain, there are no well-defined restrictions that specify which trusted websites can use your key, therefore the selected API key usage is unrestricted:

```
{
  "restrictions": {
    "browserKeyRestrictions": {
      "allowedReferrers": [
        "*.example.com"
      ]
    }
  }
}
```

* If the **services api-keys describe** command output returns one or more IPv4/IPv6 addresses for API key application restrictions, as shown in the example below, check the **"allowedIps"** array for the list of hosts that can access the selected API key. If the **"allowedIps"** is set to any host, i.e. **0.0.0.0, 0.0.0.0/0** or **::0**, there is no restriction control implemented to specify which host can use your key, therefore the selected API key usage is unrestricted:

```
{
  "restrictions": {
    "serverKeyRestrictions": {
      "allowedIps": [
        "0.0.0.0/0"
      ]
    }
  }

```

### 3 ) Enable Cloud Asset Inventory

Gaining insight into Google Cloud resources and policies is vital for tasks such as DevOps, security analytics, multi-cluster and fleet management, auditing, and governance. With Cloud Asset Inventory you can discover, monitor, and analyze all GCP assets in one place, achieving a better understanding of all your cloud assets across projects and services.

To determine if Google Cloud Asset Inventory is enabled for your GCP projects, perform the following operations:

1. Run _**projects list**_ command  with custom query filters to list the IDs of all the GCP projects available within your Google Cloud account:

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers:

```
PROJECT_ID
cc-web-project-112233
cc-mobile-project-123123
```

3. Run **services list** command which list services for a project.

```
gcloud services list  --project cc-web-project-112233  --enabled --filter=name:cloudasset.googleapis.com
```

4. The command output should return the name and the title of the requested API:

```
Listed 0 items.
```

If the _**services list**_ command output returns **Listed 0 items**, as shown in the output example above, the Cloud Asset API is currently disabled, therefore the Google Cloud Asset Inventory is not enabled for the selected GCP project.

### 4 ) Rotate Google Cloud API Keys

Once a Google Cloud API key is compromised, it can be used indefinitely unless the project owner revokes or regenerates that key. Rotating GCP API keys will substantially reduce the window of opportunity for exploits and ensure that data can't be accessed with an outdated key that might have been lost, cracked, or stolen.

To determine if your Google Cloud API keys are regularly rotated (i.e. every 90 days), perform the following operations:

1. Run _**projects list**_ command with custom query filters to list the ID of each project available in your Google Cloud account:

```
gcloud projects list  --format="table(projectId)"
```

2. The command output should return the requested GCP project IDs:

```
PROJECT_ID
  cc-project5-112233
  cc-internal-123123
  cc-web-prod-111222
```

3. Run **services api-keys list** command  using the ID of the GCP project that you want to examine as the identifier parameter and custom query filters to describe the identifier of each active API key created for the selected project:

```
gcloud alpha services api-keys list  --project=cc-project5-112233  --format="table(uid)"
```

4. The command output should return the IDs of the active API keys:

```
UID:
  abcd1234-abcd-1234-abcd-1234abcd1234
  1234abcd-1234-abcd-1234-abcd1234abcd
```

5. Run _**services api-keys describe**_ command  using the ID of the API key that you want to examine as the identifier parameter and custom query filters to describe the API key application restrictions configured for the selected key:

```
gcloud alpha services api-keys describe abcd1234-abcd-1234-abcd-1234abcd1234 --format="json(createTime)"
```

6. The command output should return the API key creation date/time:

```
CREATE_TIME: 2020-10-25T09:01:20.329336Z
```

Check the timestamp returned by the **services **_**api-keys describe**_ command output to determine when the selected API key was created. If more than 90 days have passed since the key was created, the selected Google Cloud Platform (GCP) API key is not regenerated (rotated) on a regular basis.