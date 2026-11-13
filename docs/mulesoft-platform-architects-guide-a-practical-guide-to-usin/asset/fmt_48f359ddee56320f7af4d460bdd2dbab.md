
## Monitoring the endpoints of public APIs

- `Monitor 3 (multiple schedules, multiple public locations, and multiple environments)`: `Monitor 3` runs on multiple schedules (`Schedule 4` and `Schedule 5`), and they are associated with multiple public locations (`Public Location 1` and `Public Location 2`). `Monitor 3` is set up to test public `API 5` and public `API 6` in multiple environments.

- `Monitor 4 (multiple schedules, single location, and single environment)`: `Monitor 4` runs on multiple schedules (`Schedule 6` and `Schedule 7`), and they are associated with `Public Location 2`. `Monitor 4` is set up to test public `API 7` and public `API 8` in a single environment.

- `Monitor 5 (multiple schedules, multiple locations, and single environment)`: `Monitor 5` runs on multiple schedules (`Schedule 8` and `Schedule 9`), and they are associated with multiple public locations (`Public Location 2` and `Public Location 3`). `Monitor 5` is set up to test public `API 9` and public `API 10` in a single environment.

- `Monitor 6 (multiple schedules, single location, and the World Wide Web)`: `Monitor 6` runs on multiple schedules (`Schedule 10` and `Schedule 11`), and they are associated with a single public location (`Public Location 3`). `Monitor 6` is set up to test `API 11` and `API 12`, which are available over the World Wide Web.

## Monitoring the endpoints of private APIs

Functional Monitoring allows you to monitor private endpoints that are accessible to the Anypoint Virtual Private Cloud. Private APIs are accessible within your network inside an Anypoint Virtual Private Cloud. Tests can only be run against private APIs that are accessible within the private location. Multiple private locations can be created in a single Anypoint Virtual Private Cloud and private locations that are associated with the Anypoint Virtual Private Cloud.

To set up a monitor for private endpoints running within Anypoint Virtual Private Cloud, we need to create a private location within Functional Monitoring that will create the application within Anypoint Virtual Private Cloud to monitor the private APIs. This will consume `0.2 vCores` by default. This configuration can be changed according to your requirements, such as more workers and worker size. To create a private location, select `Create new location` from the `Select location` dropdown while creating your monitor. Provide `Name` and `Environment` details for your private location:

![Figure 14.26 - Creating a private location and monitor in Functional Monitoring](image/B19922_14_26.jpg)

Figure 14.26 - Creating a private location and monitor in Functional Monitoring

Once the private location has been created, it will deploy the application in CloudHub within Anypoint Virtual Private Cloud so that you can monitor your private endpoints:

![Figure 14.27 - Private location application deployed to CloudHub](image/B19922_14_27.jpg)

Figure 14.27 - Private location application deployed to CloudHub

In Figure 14.27, we can see that the application was deployed to CloudHub within Anypoint Virtual Private Cloud so that we can monitor private endpoints. It is also capable of monitoring public endpoints.

The following diagram shows how to monitor private endpoints that have relationships with APIs, schedules, private locations, and environments:

![Figure 14.28 - Monitoring the endpoints of private APIs](image/B19922_14_28.jpg)

Figure 14.28 - Monitoring the endpoints of private APIs

The following monitors have been created in public locations for monitoring private endpoints:

- `Monitor 1 (single schedule, single private location, and single environment)`: `Monitor 1` runs on a single schedule (`Schedule 1`), which is associated with `Private Location 1`. `Monitor 1` is set up to test private `API 1` in a single environment.

- `Monitor 2 (multiple schedules, single private location, and single environment)`: `Monitor 2` runs on multiple schedules (`Schedule 2` and `Schedule 3`), which is associated with `Private Location 1`. `Monitor 2` is set up to test private `API 2` and private `API 3` in a single environment.

- `Monitor 3 (multiple schedules, multiple private locations, and single environment)`: `Monitor 3` runs on multiple schedules (`Schedule 4` and `Schedule 5`), which are associated with multiple private locations (`Private Location 1` and `Private Location 2`). Monitor 3 is set up to test public `API 5` and public `API 6` in multiple environments.

- `Monitor 4 (multiple schedules, multiple private locations, and multiple environments)`: `Monitor 4` runs on multiple schedules (`Schedule 6` and `Schedule 7`), which are associated with multiple private locations (`Public Location 1` and `Private Location 2`). `Monitor 4` is set up to test public `API 6` and public `API 7` in multiple environments.

- `Monitor 5 (multiple schedules, single private location, and single environment)`: `Monitor 5` runs on multiple schedules (`Schedule 8` and `Schedule 9`), which are associated with a single private location (`Private Location 3`). `Monitor 5` is set up to test public `API 8` and public `API 9` in a single environment.

- `Monitor 6 (multiple schedules, single private location, and the World Wide Web)`: `Monitor 6` runs on multiple schedules (`Schedule 10` and `Schedule 11`), which are associated with a single private location (`Public Location 3`). `Monitor 6` is set up to test `API 10` and `API 11`, which are available over the World Wide Web.

MuleSoft provides options so that you can monitor private endpoints from public locations. This can be easily achieved by adding a firewall rule to Anypoint Virtual Private Cloud.

In the Anypoint Virtual Private Cloud firewall rule, `Type` must be set to `https.port`, `Source` can be customized, and `Port Range` must be `8082`. For the custom source, add one of the following IP addresses shown in Table 14.2, depending on your public location:

![Figure 14.29 - Anypoint Virtual Private Cloud firewall rules for monitoring private endpoints](image/B19922_14_29.jpg)

Figure 14.29 - Anypoint Virtual Private Cloud firewall rules for monitoring private endpoints

With this approach, you don't have to create a private location to monitor private endpoints. This will result in you saving the vCores that are required for running the application so that you can monitor private endpoints from private locations:

| Public Location | IP Address |
| --- | --- |
| us-east-1 | 3.82.89.58 |
| us-east-2 | 3.128.151.139 |
| eu-central-1 | 18.156.106.167 |

Table 14.2 - IP addresses (Public Location)

Table 14.2 shows various IP addresses for public locations.

# Why is logging important?

Logging is important for any application for event and message tracing, troubleshooting, measuring application performance, and identifying errors that occur in an application. Logging is crucial for any developer and operational person to identify and troubleshoot issues quickly.

MuleSoft provides application and runtime logs to troubleshoot issues related to the application and the Mule runtime. The logger component can be used in your applications for monitoring and troubleshooting. It logs error information, payloads, and any other information. This component can be used anywhere in the flow to log strings or dataweave expressions, or a combination of strings and dataweave expressions.

## Logs in Anypoint Monitoring
