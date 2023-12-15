# CHART
* To create helm chart. This command creates a chart directory along with the common files and directories used in a chart.
```
helm create <chart_name>
```

# RELEASES
* To install a chart
```
helm install <release_name> <chart_name>
```

* To pass a value to the variable defined in values.yaml file while installing the chart
```
helm install <release_name> --set <variable_name-1>="<value-1>" --set <variable_name-2>="<value-2>" <chart_name>
```

* To pass a custom value file while installation a chart
```
helm install <release_name> --values <custom-values.yaml> <chart_name>
```
Values from custom-values.yaml file will overwrite the values in Values.yaml file.

* To install specific version of helm chart
```
helm intall <release_name> <chart_name> --version <chart_version>
```

# UPGRADE

* To upgrade the chart
```
helm upgrade <release_name> <chart_name>
```

* To upgrade the chart to sepcific chart version
```
helm upgrade <release_name> <chart_version>
```

# REVISION

* To check the releases use command 
```
helm list
```

* To get detailed hisotry(all revisions and detials) about a chart
```
helm history <release_name>
```

# ROLLBACK

* To go back to specific revision
```
helm rollback <release_name> <revision_number>
```

# TEMPLATING

* Using helm Release details inside the K8 yaml file.
```
{{ .Release.Name}}
``` 
```
{{ .Release.Namespace}}
``` 
```
{{ .Release.IsUpgrade}}
``` 
```
{{ .Release.IsInstall}}
``` 
```
{{ .Release.Revision}}
``` 
```
{{ .Release.Service}}
``` 

* To get chart details we mentioned in chart.yaml file inside K8 yaml file.
```
{{ .Chart.Name }}
```
```
{{ .Chart.ApiVersion }}
```
```
{{ .Chart.Version }}
```
```
{{ .Chart.Type }}
```
```
{{ .Chart.Keywords }}
```
```
{{ .Chart.Home }}
```
* To get details about K8 cluster use Capabilities object
```
{{ .Capabilities.KubeVersion }}
```
```
{{ .Capabilities.ApiVersions }}
```
```
{{ .Capabilities.HelmVersion }}
```
```
{{ .Capabilities.GitCommit }}
```
```
{{ .Capabilities.GitTreeState }}
```
```
{{ .Capabilities.GoVersion }}
```
* To get values specified in Values.yaml file use Values object.
```
{{ .Values.<variable> }}
```