## Accessing Prometheus

The prometheus built in expression editor can show metrics and build simple
graphs, alerts and configs. This useful for testing and debugging queries, but
is not intended to be used as a dashboard.

To access the prometheus built-in expression editor:

```
kubectl get pods -l app=prometheus -o name | cut -d/ -f2 | xargs -I{} kubectl port-forward {} 9090:9090
```

If you want to view grafana, the dashboard we're using to graph prometheus
metrics, we can use the kubernetes proxy like so:

```
kubectl proxy
```

And then you can access the dashboard at
http://localhost:8001/api/v1/proxy/namespaces/prometheus/services/grafana/


## TODOS:

- Create a custom Grafana docker image
    - Pre-install dashboards in this repos `grafana/dashboards` directory by
      baking them into the image, and putting them into the grafana dashboards
      directory within the image.
    - Create a custom grafana config file which sets dashboards.json.enabled to
      true
    - Find a way to create a pre-configured data source so we can have
      prometheus configured by default.
        - There seems to be an API for this, so this might involve a custom
        entrypoint in the container which starts grafana and adds the
        datasource when grafana starts.
        - https://github.com/grafana/grafana/issues/2977
