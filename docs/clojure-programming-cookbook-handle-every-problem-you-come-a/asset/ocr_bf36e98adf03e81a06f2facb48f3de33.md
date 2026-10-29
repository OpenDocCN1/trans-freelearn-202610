The following snippet follows the previous bullet points:

```
(require  '[monger.core :as mg]
            '[monger.collection :as mc]
            '[monger.operators :refer :all])

(defn endpoint [city]
  (str
    "http://api.openweathermap.org/data/2.5/weather?q="
    city
    "&APPID=<KEY>&units=metric"))

(defn load-weather-data [city]
  (-> (get cl (endpoint city) {:as :json})
      <!
      :body
      <!))

(defn store-data-to-mongo [city]
  (mc/insert
    db
    "weather"
    (load-weather-data city)))

(When #"I load weather data for the following cities:$" [table]
  (mc/drop db "weather")
  (doseq [city (.raw table)]
    (store-data-to-mongo (first city))))

(Then #"^I expect the average to be lower than (\d+)$"
  [maximum]
  (let [
    coll "weather"
    average
      (:avg (first (mc/aggregate db coll [
        {$group {:_id "total" :avg {$avg "$main.temp"}}}]))) 
  ]
  (assert (< average (bigdec maximum) ))))
```

The BDD framework now helps you go beyond just testing your own code, but also makes interacting with the environment, and the goals of the feature, clear to everyone. It communicates the intention, and allows any other average user to understand the goal, and also be able to