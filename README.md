# traffic
Visualisasi Lalu lintas kota-kota di Indonesia dengan Leaflet

### Data
untuk pengumpulan data saya menggunakan bantuan python dan pythoneverywhere

```python
from urllib.request import urlopen
import json
import pickle
from datetime import datetime as dt
from datetime import timedelta

def get_data(): 
    appkey = "APP KEY"
    base = f"https://traffic.ls.hereapi.com/traffic/6.2/flow.json"+\
    "?apiKey={appkey}"+\
    "&bbox=-6.775616,111.906528;-8.162503171095738,113.70357436384134&responseattributes=sh,fc"

    response = urlopen(base)
    data = json.load(response)

    time = data["CREATED_TIMESTAMP"][0:19].replace("T", " ")    
    time = dt.strptime(time, '%Y-%m-%d %H:%M:%S') + timedelta(hours = 7)
    nama = str(time).replace(":", "-") + ".p"
    
    with open(nama, "wb") as f:
        json.dump(data, f)
        # pickle.dump(data, f, protocol=pickle.HIGHEST_PROTOCOL)


from apscheduler.schedulers.blocking import BlockingScheduler

scheduler = BlockingScheduler()
# ganti dengan tanggal start
scheduler.add_job(get_data, 'interval', minutes=30, next_run_time=dt(2020, 2, 18, 17, 0, 0))
scheduler.start()
```

### Jakarta
![Alt Text](https://github.com/Alfrzlp/traffic/blob/main/hasil/jakarta.gif)

### Surabaya
![Alt Text](https://github.com/Alfrzlp/traffic/blob/main/hasil/surabaya_.gif)
