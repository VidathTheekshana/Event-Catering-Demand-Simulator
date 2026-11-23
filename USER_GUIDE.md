# User guide: Event Catering Demand Simulator web demo

**Open the demo:** [https://VidathTheekshana.github.io/event-catering-demand-simulator/](https://VidathTheekshana.github.io/event-catering-demand-simulator/). It runs entirely in your browser (current Chrome, Edge or Firefox). Nothing is installed or uploaded.

> Labelled screenshot: add a browser capture at `figures/demo-screenshot-labelled.png` before publishing. This file is not included because browser capture was unavailable during the audit.

## Controls

| Control | What it does | Range | Default |
|---|---|---|---|
| Invited guests N | guests invited to the event | 50 to 500 | 200 |
| Attendance p | probability each invited guest attends | 0.50 to 1.00 | 0.90 |
| Prepared portions Q | food prepared before the event (the decision) | 50 to 800 | 285 |
| Serving stations | parallel buffet service points | 1 to 8 | 4 |
| Arrivals λ | guest arrival rate (guests per minute) | 1 to 12 | 5 |
| Service μ | service rate of each station (guests per minute) | 0.5 to 4 | 1.5 |
| Prep cost / portion | cost of each prepared portion (LKR) | 100 to 800 | 300 |
| Shortage penalty / portion | penalty per unmet portion (LKR) | 300 to 6,000 | 1,500 |
| Speed | simulated minutes per real second | 1 to 40 | 8 |
| Random seed | makes runs repeatable | any integer | 42 |
| Deterministic test | every guest takes exactly 2 portions | on/off | off |
| Start / Pause | runs or pauses the animated event | – | – |
| Reset | clears the run; new settings apply on the next Start | – | – |
| Run 10,000 events | batch of 10,000 independent events | – | – |

## Reading the outputs

- **Buffet picture:** yellow dots are guests waiting in the queue; teal boxes are busy serving stations; the bar at the bottom is the food remaining (it turns gold when nearly empty and red once guests have gone unserved).
- **Live numbers:** simulated clock in minutes, guests attending, queue length, portions left and unmet portions.
- **Event summary:** when an event ends, the banner reports requested, served, unused and unmet portions, total cost and mean wait.
- **Batch table:** mean ± 95% confidence interval for unused portions, shortage probability, total cost and mean queue wait over 10,000 events.

## Try this

1. Load **reference A** (N=200, p=0.9, Q=285) and press **Run 10,000 events**. Shortage probability is about 9%.
2. Raise Q to 290 and run again: shortage probability falls to about 5% while unused food rises. This is the trade-off at the heart of the model.
3. Drop to 2 serving stations and press Start: the queue grows and waits lengthen, but the food outcome is unchanged. Queue delay and food shortage are separate problems.
4. Load **reference B** (the deterministic test): demand is exactly 100, so 90 served, 10 unmet and 0 leftover every time.

## Check against the notebook

With reference A the demo should give, within 3 combined standard errors (the browser and notebook use different random-number streams):

| Output | Notebook reference (section 5.7) |
|---|---|
| Mean unused portions | 15.58 ± 0.21 |
| P(shortage) | 0.093 ± 0.006 |
| Mean total cost (LKR) | 86,330 ± 66 |
| Mean wait in queue (min) | 0.491 ± 0.008 |

The batch table shows PASS or FAIL for each. Because four checks run at once, an occasional FAIL can occur by chance (about 1%); try another seed.

## Limitations

- The portion probabilities (0.05, 0.50, 0.35, 0.10) are fixed in the demo; the notebook also studies other consumption patterns and a multi-category menu.
- All parameters are assumed values, not measurements of any real caterer.
- Results are reliable only while the buffet queue is stable (arrival rate below serving capacity, λ < c × μ); very overloaded settings give long queues and unrealistic waiting times.
