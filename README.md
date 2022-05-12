### Export metrics to Azure Application Insight

* Prerequisites
  - Set up [Application Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/create-new-resource)
  - Set up WLS on AKS cluster

* Integrate Application Insight

  You can follow [Java codeless application monitoring Azure Monitor Application Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/java-in-process-agent#quickstart) to integrate App Insight, here lists the main steps.

  - Upload [applicationinsights-agent-3.1.1.jar](resources/applicationinsights-agent-3.1.1.jar) to file share `app-insight/applicationinsights-agent-3.1.1.jar`
  - Copy connection string from Application insight overview page
  - Add `-javaagent:/shared/app-insight/applicationinsights-agent-3.1.1.jar` to your domain JVM args
  - Create Env `APPLICATIONINSIGHTS_CONNECTION_STRING` in domain level in domain configuration, set its value with Application insight connection string.
  - 
      ```YAML
      env:
      - name: JAVA_OPTIONS
        value: "-javaagent:/shared/app-insight/applicationinsights-agent-3.1.1.jar"
      - name: APPLICATIONINSIGHTS_CONNECTION_STRING
        value: InstrumentationKey=9f4e489d-b040-4a35-a76c-efd1111;IngestionEndpoint=https://eastus-8.in.applicationinsights.azure.com/
      ```

  - Apply the configuration, and wait for domain restart completed. See the sample YAML in [custom-domain.yaml](custom-domain.yaml)

      ```
      kubectl apply -f custom-domain.yaml
      ```
