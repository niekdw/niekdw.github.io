---
title: "Extract highlights from a Kobo ereader"
date: 2022-07-23T21:07:49+02:00
---

```
import pandas as pd
import sqlite3

ereader_path = "/media/niek/KOBOeReader/.kobo"
db = ereader_path + "/KoboReader.sqlite"

con = sqlite3.connect(db)
query = con.execute("SELECT * From Bookmark")

cols = [column[0] for column in query.description]
df = pd.DataFrame.from_records(data=query.fetchall(), columns=cols)

highlights = df.iloc[0:1000, [1, 9]]

highlights["VolumeID"] = highlights["VolumeID"].str.replace("file:///mnt/onboard/", "")
highlights["VolumeID"] = highlights["VolumeID"].str.replace(".epub", "")

highlights["Text"] = highlights["Text"].str.replace("\n", "")
highlights["Text"] = highlights["Text"].str.replace("\\", "")

df = highlights.groupby("VolumeID")
df2 = df.apply(lambda x: x["Text"].unique())

df2.to_json("highlights.json", indent=2)
```