### Agregated report
```

apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: {{ include "kubecost-reports-exporter.fullname" . }}-aggregated
  labels:
    {{- include "kubecost-reports-exporter.labels" . | nindent 4 }}
spec:
  schedule: {{ .Values.schedule | quote }}
  successfulJobsHistoryLimit: {{ .Values.successfulJobsHistoryLimit }}
  concurrencyPolicy: {{ .Values.concurrencyPolicy }}
  jobTemplate:
    spec:
      template:
        metadata:
          {{- with .Values.podAnnotations }}
          annotations:
            {{- toYaml . | nindent 12 }}
          {{- end }}
          labels:
            {{- include "kubecost-reports-exporter.labels" . | nindent 12 }}
        spec:
          {{- with .Values.imagePullSecrets }}
          imagePullSecrets:
            {{- toYaml . | nindent 12 }}
          {{- end }}
          securityContext:
            {{- toYaml .Values.podSecurityContext | nindent 12 }}
          serviceAccountName: {{ include "kubecost-reports-exporter.serviceAccountName" . }}
          containers:
          - name: {{ .Chart.Name }}-aggregated
            securityContext:
              {{- toYaml .Values.securityContext | nindent 14 }}
            image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart.AppVersion }}"
            imagePullPolicy: {{ .Values.image.pullPolicy }}
            args:
              - cost-exporter
            envFrom:
              - secretRef:
                  name: {{ include "kubecost-reports-exporter.fullname" . }}
            env:
              - name: LOG_LEVEL
                value: {{ .Values.kubecost.logLevel | quote }}
              - name: BUCKET_NAME
                value: {{ required " Aws s3 bucket name is required value"  .Values.kubecost.bucketName | quote }}
              - name: CLUSTER_NAME
                value: {{ required "Cluster name is a required value"  .Values.kubecost.clusterName | quote }}
              - name: REPORT_TYPE
                value: "aggregated"
              - name: KUBECOST_ENDPOINT
                value: {{ required "Kubecost analyzer endpoint is a required value" .Values.kubecost.analyzerEndpoint | quote }}
              - name: KUBECOST_URL
                value: {{ required "Kubecost reports url is a required value" .Values.kubecost.aggregatedCostUrl  | quote }}
          restartPolicy: {{ .Values.restartPolicy }}

```