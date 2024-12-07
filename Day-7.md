Today we learn to use `JQ` to filter out `AWS` logs. We leanr to use `jq` to filter for particular info which will help us analyze what went wrong in the `flyer`.
+ Firstly we do `cd wareville_logs` to get into the directory that contains the `json` files which are to be analyzed.
+ Now typing the following command will give us valuable info:
  ```
  ubuntu@tryhackme:~/wareville_logs$ jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"], (.Records[] | select(.sourceIPAddress=="53.94.201.69") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'
  Event_Time            Event_Source                         Event_Name               User_Name      Source_IP
  2024-11-28T15:20:38Z  s3.amazonaws.com                     HeadBucket               mayor_malware  53.94.201.69
  2024-11-28T15:22:12Z  s3.amazonaws.com                     HeadBucket               glitch         53.94.201.69
  2024-11-28T15:22:23Z  s3.amazonaws.com                     ListObjects              glitch         53.94.201.69
  2024-11-28T15:22:25Z  s3.amazonaws.com                     ListObjects              glitch         53.94.201.69
  2024-11-28T15:22:39Z  s3.amazonaws.com                     PutObject                glitch         53.94.201.69
  2024-11-28T15:22:39Z  s3.amazonaws.com                     PreflightRequest         N/A            53.94.201.69
  2024-11-28T15:22:44Z  s3.amazonaws.com                     ListObjects              glitch         53.94.201.69
  2024-11-28T15:18:37Z  signin.amazonaws.com                 ConsoleLogin             mayor_malware  53.94.201.69
  2024-11-28T15:20:54Z  signin.amazonaws.com                 ConsoleLogin             mcskidy        53.94.201.69
  2024-11-28T15:21:54Z  signin.amazonaws.com                 ConsoleLogin             glitch         53.94.201.69
  2024-11-28T15:21:26Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:29Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:30Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:30Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:30Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:31Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:31Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:30Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:25Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:31Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:31Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:31Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:31Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:33Z  iam.amazonaws.com                    GetPolicy                mcskidy        53.94.201.69
  2024-11-28T15:21:33Z  iam.amazonaws.com                    GetPolicy                mcskidy        53.94.201.69
  2024-11-28T15:21:31Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:33Z  iam.amazonaws.com                    GetPolicy                mcskidy        53.94.201.69
  2024-11-28T15:21:33Z  iam.amazonaws.com                    GetPolicy                mcskidy        53.94.201.69
  2024-11-28T15:21:32Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:32Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:36Z  iam.amazonaws.com                    CreateLoginProfile       mcskidy        53.94.201.69
  2024-11-28T15:21:36Z  iam.amazonaws.com                    AttachUserPolicy         mcskidy        53.94.201.69
  2024-11-28T15:21:32Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:32Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:32Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:32Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:32Z  iam.amazonaws.com                    ListPolicies             mcskidy        53.94.201.69
  2024-11-28T15:21:44Z  iam.amazonaws.com                    ListUsers                mcskidy        53.94.201.69
  2024-11-28T15:21:35Z  iam.amazonaws.com                    CreateUser               mcskidy        53.94.201.69
  2024-11-28T15:21:45Z  organizations.amazonaws.com          DescribeOrganization     mcskidy        53.94.201.69
  2024-11-28T15:21:57Z  ce.amazonaws.com                     GetCostAndUsage          glitch         53.94.201.69
  2024-11-28T15:21:57Z  cost-optimization-hub.amazonaws.com  ListEnrollmentStatuses   glitch         53.94.201.69
  2024-11-28T15:21:57Z  health.amazonaws.com                 DescribeEventAggregates  glitch         53.94.201.69
  2024-11-28T15:22:12Z  s3.amazonaws.com                     ListBuckets              glitch         53.94.201.69
  2024-11-28T15:21:57Z  health.amazonaws.com                 DescribeEventAggregates  glitch         53.94.201.69
  2024-11-28T15:21:57Z  ce.amazonaws.com                     GetCostAndUsage          glitch         53.94.201.69
  2024-11-22T11:08:03Z  signin.amazonaws.com                 ConsoleLogin             mayor_malware  53.94.201.69
  2024-11-23T07:19:01Z  signin.amazonaws.com                 ConsoleLogin             mayor_malware  53.94.201.69
  2024-11-24T02:28:17Z  signin.amazonaws.com                 ConsoleLogin             mayor_malware  53.94.201.69
  2024-11-25T21:48:22Z  signin.amazonaws.com                 ConsoleLogin             mayor_malware  53.94.201.69
  2024-11-26T22:55:51Z  signin.amazonaws.com                 ConsoleLogin             mayor_malware  53.94.201.69
  ```
  + From this particular command we can see that three usernames were accessed from the same `ip` of `53.94.201.69`
## What is the other activity made by the user glitch aside from the ListObject action?
We can see that glitch did `PutObject`
## What is the source IP related to the S3 bucket activities of the user glitch?
The ip through which the user `glitch` was accessed from is `53.94.201.69`
## Based on the eventSource field, what AWS service generates the ConsoleLogin event?
The corresponding `eventSource` to `ConsoleLogin` is `signin.amazonaws.com`.
## When did the anomalous user trigger the ConsoleLogin event?
We can see that the user triggered ConsoleLogin event on `2024-11-28T15:21:54Z`
## What was the name of the user that was created by the mcskidy user?
The user created by `mcskidy` is `glitch`
## What type of access was assigned to the anomalous user?
We can see that `AdministratorAccess` was provided to the anomalous user.
![image](https://github.com/user-attachments/assets/e27f35b7-a988-4c73-af3b-d27ea7414ee7)
## Which IP does Mayor Malware typically use to log into AWS?
The ip is `53.94.201.69`
## What is McSkidy's actual IP address?
Executing the following command and scrolling down we can see that the ip of the windows machine which mcskidy uses is `31.210.15.79`
```
ubuntu@tryhackme:~/wareville_logs$ jq -r '["Event_Time","Event_Source","Event_Name", "User_Name","User_Agent","Source_IP"],(.Records[] | select(.userIdentity.userName=="mcskidy") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A",.userAgent // "N/A",.sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'
```
![image](https://github.com/user-attachments/assets/3a55f57c-9dc4-4b8e-91b6-d2bab953f943)

## What is the bank account number owned by Mayor Malware?
Executing the follwoing command we can see that `2394 6912 7723 1294` is Mayor Malware's account number
```
ubuntu@tryhackme:~/wareville_logs$ grep INSERT rds.log
```
![image](https://github.com/user-attachments/assets/ef5e7091-d72d-45f4-a965-56f4c4624d59)
