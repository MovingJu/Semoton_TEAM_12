# 2025_TEAM_12

![우수상 이미지 대체 텍스트](statics/images/result.png)

**LinKHU**

세모톤 12팀 코드

우수상 수상작

# Contributers

- **Repository Management** : [@Movingju](https://github.com/Movingju)
- **Design** : [@lyksunny1214](https://github.com/lyksunny1214)
- **Frontend** : [@ParkingLot0326](https://github.com/ParkingLot0326), [@seohyunlee-coding](https://github.com/seohyunlee-coding), [@moolzoo](https://github.com/moolzoo)
- **Backend** : [@Movingju](https://github.com/Movingju), [@scythe0425](https://github.com/scythe0425)

# Repository

- **Backend** : All other files/folders except [khu-map-front](/khu-map/)
- **Frontend** : In [khu-map-front](/khu-map/)

# What does this repo do?

We wrote main features of LinKHU in [presentation pdf](/presentation.pdf)

# Tech stack

- **Backend** : Python3.10, Flask, Sqlite
- **Frontend** : React, Html, Css, Javascript

# Python setup

Linux

```bash
python3.10 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Windows

```powershell
python -m venv venv
.\venv\Script\activate
pip installl -r requirements.txt
```

# How to run

In one console

```bash
python3 main.py
```

In another console

```bash
cd khu-map
npm install
npm run dev
```

And the server will run on http://localhost:5173/

# FAQ

## How Frontend and Backend communicates?

We created a backend with endpoints.

You can check endpoints and funcitons in [BACKEND README](/README_backend.md).

## Why hasn't pathfinding feature been implemented?

The backend includes the pathfinding feature.

But we didn't have enough time to implement it on the frontend.

You can check simple pathfinding feature by entering following code:

```yaml
http://localhost:5000/navigate?start=전정대&end=멀관
```

on your chrome while the server is running

# License

This repo has a MIT license as found in the [LICENSE](/LICENSE) file.
