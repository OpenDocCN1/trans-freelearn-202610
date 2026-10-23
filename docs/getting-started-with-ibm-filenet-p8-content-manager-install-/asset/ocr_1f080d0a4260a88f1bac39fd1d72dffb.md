After deploying the new XT version, perform the same file ownership tweaks as we did after deploying the CE server earlier in this chapter. There is an additional step for the XT log file location. Make sure that it is writable by the operating system user and group under which the WAS profile is running.

```
# chown ecmuser:ecmgroup /opt/ibm/FileNet/WebClient/LogFiles
```

Use your web browser to connect to XT at http://wjc-rhel.example.net:9080/ WorkplaceXT/. You should be able to log in and perform all operations as before.

# Troubleshooting

Sooner or later, your perfectly happy world of ECM will stumble a bit, and your goal will be to get things working as soon as possible. If trouble happens in your development or test environment, you'll be uneasy with the lost productivity until you can use your full setup again. If trouble happens in your production environment, you might rapidly move beyond being in a panic.

We could present quite a long list of specific things to check or try when you run into problems. That could fill a book of this size by itself, and we might not happen to cover the circumstances of your particular situation. We will instead describe some general troubleshooting techniques that may help you zero in on the problem.

# Prevention

If you are reading this chapter because your production system is down, please skip this section. We don't want to risk you becoming violent when we tell you that the best troubleshooting technique is to avoid trouble in the first place. It's true, of course. Despite the best intents of a disciplined process, sooner or later you might be able to subvert one of these obviously good ideas.

# Configuration Control

Configuration control is essential for a smooth running production environment, but it's also a good idea for important non-production environments. Don't make changes without recording them in a reliable way. Here, reliable can mean anything from rigorous note-taking to a full-fledged software configuration management system. Some sites have a policy that no change can be made to a production environment unless it is scripted. Nothing is left to human hands except running the script itself.