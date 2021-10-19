# traffic
Visualisasi Lalu lintas kota-kota di Indonesia dengan Leaflet

### Data
untuk pengumpulan data saya menggunakan bantuan python dan python anywhere

```python
from urllib.request import urlopen
import json
import pickle
from datetime import datetime as dt
from datetime import timedelta
from apscheduler.schedulers.blocking import BlockingScheduler

def get_data(): 
    appkey = "APP KEY"
    # surabaya
    base = f"https://traffic.ls.hereapi.com/traffic/6.2/flow.json"+\
    "?apiKey={appkey}"+\
    "&bbox=-6.775616,111.906528;-8.162503171095738,113.70357436384134&responseattributes=sh,fc"
    
    # bbox jakarta
    # -6.077811,106.559667;-6.665355,107.146117
    
    response = urlopen(base)
    data = json.load(response)

    time = data["CREATED_TIMESTAMP"][0:19].replace("T", " ")    
    time = dt.strptime(time, '%Y-%m-%d %H:%M:%S') + timedelta(hours = 7)
    nama = str(time).replace(":", "-") + ".p"
    
    with open(nama, "wb") as f:
        json.dump(data, f)
        # pickle.dump(data, f, protocol=pickle.HIGHEST_PROTOCOL)


scheduler = BlockingScheduler()
# ganti dengan tanggal start
scheduler.add_job(get_data, 'interval', minutes=30, next_run_time=dt(2020, 2, 18, 17, 0, 0))
scheduler.start()
```

Untuk menyimpan plot anda bisa menggunakan cara lain
seperti save dalam html terlebih dahulu lalu screenshot dengan Rselenium
```r
library(htmlwidgets)
saveWidget(map, "2020-2-18-01-00-00.html")


library(RSelenium)
library(wdman)

# sesuai versi chrome
cDrv <- chrome(version = "88.0.4324.27")
remDr <- remoteDriver(browserName = "chrome", port = 4444L)
remDr$open()
remDr$navigate("2020-2-18-01-00-00.html")
remDr$screenshot(file = '2020-2-18-01-00-00.png')

# clean up
remDr$close()
cDrv$stop()
```

## Hasil
### Jakarta
![Alt Text](https://github.com/Alfrzlp/traffic/blob/main/hasil/jakarta.gif)

### Surabaya
![Alt Text](https://github.com/Alfrzlp/traffic/blob/main/hasil/surabaya_.gif)


## Referensi
- [Lalu Lintas Jakarta di Pekan Kemerdekaan](https://medium.com/@nmonarizqa/lalu-lintas-jakarta-di-pekan-kemerdekaan-2f0d67c23240)
