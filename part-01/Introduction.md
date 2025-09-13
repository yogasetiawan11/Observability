# What is Observability
Observability is a practice that allows you to understand the internal state of a system by examine its external outputs. It provides the ability to ask questions about a system. which is crucial for troubleshooting complex, modern applications.

In this day where the application grow in complexity and distributed software systems. understanding the internal state and behavior of an application is crucial for Provide better experience to deliver application to end users, maintaining performance of the system, and reliability. This is why observability comes into play. 

In short If you have observability setup you can get the Information of internal system such as (application, infrastructure, and networking) with this Information you will get an Insight of your system.

<img width="1500" height="1500" alt="Image" src="https://github.com/user-attachments/assets/fbc8c1ca-2aca-454c-99c2-525f913ee348" />

# There are 3 pillar of Observability (Metric, Logs, and Traces)
# Metric
- ```Metric has responsibility to understanding "What is the state of an application"``` 
Metric basically give you historical data of event this event can be CPU, Memory, Disk, Http request. and historical means you can get the event over the last time or the event relating to the past.

# Logging (Logs)
- ```Logging is responsible "Why your system in this particular state"```
Logging is the practice of recording events that happen within a software application or system. The primary goal of logging is to provide a detailed, time-stamped record that helps developers and operators answer the question: "Why is the system in this particular state?" 
# Traces
- ```Traces can help "How to fix particular state```
If you have Traces along with the Logs, so Logs will give you some of information, whereas Traces will give you extensive information to debug, troubleshoot as well as fix your issues.

# The difference between Monitor and Observability
Observability covers 3 pillars that is Metrics, Logs, and Traces. on the other side Monitoring only cover 1 pillar which is Metrics, along with Metrics if you have alert and Dashboard. if you have these things basically monitoring part. you can record these information in dasboard like grafana.