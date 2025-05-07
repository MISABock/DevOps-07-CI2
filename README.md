# DevOps 07 CI2

## Lernjournal

Als erstes habe ich den Shell-Befehle für Sonar Analyse ins CMD eingeben damit das Project auf Sonar verfügbar ist und analysiert wird. (![SonarBackendCMD](images/SonarBackendCMD.png)) (![SonarBackendLocalhost](images/SonarBackendLocalhost.png))

Das gleiche fürs Frontend: (![SonarFrontendCMD](images/SonarFrontendCMD.png)) (![SonarFrontendLLocalhost](images/SonarFrontendLLocalhost.png))


Als nächstes habe ich auf Jenkins eine Pipeline angelegt: (![JenkinsPipeline](images/JenkinsPipeline.png)) (![JenkinsPipileConf](images/JenkinsPipileConf.png))

Mit einem Build dann geprüft ob alles funktioniert soweit: (![PipelineBuildCheck](images/PipelineBuildCheck.png))

Als nächstes habe ich eine neue Cloud erstellt: (![JenkisnCloud](images/JenkisnCloud.png)) (![JenkinsCloudConfig](images/JenkinsCloudConfig.png))

Auf folgenden Logs sieht man, dass die Applikation auf der Jenkins Cloud läuft: (![RunningOnJenkinsCloud](images/RunningOnJenkinsCloud.png) 

Auch hier bei der Pipelien sieht man, dass Jenkins erfolgreich ein Docker Agent mit dem Image jenkins/agent:jdk21 gestartet und verbunden hat. Gleichzeitig sieht man, dass die Verbindung zum meinem Repo funktioniert.  (![jenkinsLogsImageSuccess](images/jenkinsLogsImageSuccess.png))

Desweiteren musste ich den Token von Sonar in die Umgebung eingeben damit das frontend und backend laufen kann. (![TokensSonar](images/TokensSonar.png)

auf folgendem Screenshot sieht man wie das Gradle erfolgreich über die Pipeline gebaut wurde: (![pipelineBuild](images/pipelineBuild.png))

auf folgendem Screenshot das Jacocoplugin und Bericht (![jacocoPipelineLogs](images/jacocoPipelineLogs.png))

Das selbe für das Frontend Linting  (![FrontendLinting](images/FrontendLinting.png))

Das Backend wurde auch richtig gestaret: (![PipelineBackendLogs](images/PipelineBackendLogs.png)) 
PipelineFrontedLogs
letztlich wurde auch das frontend erfolgreich gestartet: (![PipelineFrontendLogs](images/PipelineFrontendLogs.png)) 

Letztlich wurde das auf einem neuem Node gestarted, nicht Master Node: (![NodeDocker](images/NodeDocker.png)) 
