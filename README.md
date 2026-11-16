# Paperspace alternatives: what the $8 and $39 plans really cost, which GPU clouds undercut $3.18/hr on an A100, and when staying put is cheaper

Paperspace's paid floor is **$8/month for Pro or $39/month for Growth**, and GPU time bills on top: **$3.18/hr for an A100-80G, $5.95/hr for an H100, $47.60/hr for an H100 x8**. If the subscription is the part you want gone, an A100-80G now rents for $1.09 to $1.99/hr on platforms that charge nothing monthly, and at 40 hours a month that spread is worth more than the plan fee itself.

Below: what the plan-plus-hourly structure costs at different volumes, the Vultr lineup with the hardware that is actually confirmed, five setups and which platform fits each, and the cases where staying on Paperspace is the right answer.

> **Quick pick.** If you want a GPU by the hour and somewhere to run the API, the Postgres instance and the queue in the same account, price Vultr first. Its Cloud GPU runs in 34 data centers across six continents, the H100 is there at 80GB HBM3 and the A100 at 80GB in PCIe or SXM form, and Vultr Clusters will stand up a multi-node GPU cluster on InfiniBand scheduled with Slurm or Kubernetes without a reservation.
> 👉 [Browse Vultr's Cloud GPU options](https://bit.ly/VuLtr)

## What Paperspace actually charges in 2026

Paperspace now sits inside DigitalOcean, and the four products are still there: Notebooks (a browser Jupyter IDE), Deployments, Workflows, and Machines, which are Linux VMs with GPU options. The first three, plus the subscription plans, still carry the Gradient name.

Two prices stack. The plan is Free, Pro at $8/month or Growth at $39/month, and each bundles free notebook machines: an A4000 on Pro, an A6000 and an A100-80G on Growth. Paid machines bill hourly, per DigitalOcean's Paperspace pricing page:

- A6000: **$1.89/hr**
- A100-80G: **$3.18/hr**
- H100: **$5.95/hr**
- H100 x8: **$47.60/hr**, exactly eight times the single-card rate. The x8 machine supports NVLink; the x1 does not.

Multi-GPU machines exist across the A6000, V100-32G, A100, A100-80G and H100, so an 8-card box is available without a sales call. What is not available is anything beyond a single 8-GPU machine, and that matters if your job needs a scheduler across nodes.

The free machines carry restrictions that decide more migrations than the price does:

> Free machines shut down automatically after 6 hours, notebook machines shut down after 1 hour idle, only one notebook can use a free machine at a time, and on the Free plan the free machines are always public.

Public is the one people miss. A free A100-80G on Growth is useful hardware, but it is not somewhere to put a proprietary dataset, credentials, or a customer's fine-tuning data.

Two billing details work in Paperspace's favour. Machines include unlimited bandwidth, and shutting a machine down stops compute billing. Plans and storage carry monthly caps, so a runaway experiment cannot bill indefinitely. Windows templates were retired for new users in July 2024, so a new account only gets Linux machines.

One claim worth knowing, from packet.ai's August 2026 comparison: it states that the A100-80G and H100 on-demand rates require the $39 Growth plan, that Paperspace runs in three regions (NY2, CA1 and AMS1), and that there is no spot pricing. The subscription gate is a reported experience rather than something DigitalOcean's pricing page spells out, so verify it on your own account. The three-region point is the one to act on if latency or data residency is part of your decision, because three regions is a narrow footprint next to what most GPU clouds now run.

## The plan is a floor, not a discount

Divide the plan price by the hourly rate for the same card elsewhere and you get the number of hours per month at which the plan stops costing you money and starts saving it. Using Nodus's published rates as the comparison, which is its own number and should be read as such:

- Pro at $8/month ÷ $0.173/hr RTX A4000 = **46.2 hours/month**
- Growth at $39/month ÷ $1.090/hr A100 80GB = **35.8 hours/month**
- Growth at $39/month ÷ $0.333/hr RTX A6000 = **117.1 hours/month**

Swap in RunPod's $1.79/hr Secure A100 80GB and the Growth break-even falls to 21.8 hours a month. The shape holds regardless of which provider you divide by: under those hours you are paying for capacity you don't use, over them the bundled machine is doing real work.

Run the exercise once with your own numbers before you migrate anything. Someone running an A100 for six hours a month is better off renting six hours anywhere at list price than paying $39 for the privilege, even though the Growth plan technically includes an A100-80G. Someone running 60 hours a month has the opposite problem: the plan is already paid for, and the question becomes whether the free machine's 6-hour session cap and public visibility can survive their workload at all.

## The Vultr lineup, and which row you need

Vultr sells compute in four product lines that matter for this decision. GPU rates are set per card and per configuration on Vultr's price list, so confirm the figure on the plan you pick before ordering; the entry prices marked below are third-party aggregates, not quotes from Vultr's own list.

| Product line | Hardware and what you get | Entry price | Billing | Best for | Start here |
| --- | --- | --- | --- | --- | --- |
| Cloud Compute | Shared or dedicated-CPU instances across regular, high-frequency and high-performance families | about $2.50/mo* | Hourly on-demand or monthly | A Jupyter host, API server or queue worker sitting next to the GPU box | [Start with Cloud Compute](https://bit.ly/VuLtr) |
| Optimized Cloud Compute | Dedicated CPU in CPU-, memory- or storage-optimized shapes | about $28/mo* | Hourly on-demand or monthly | Postgres, Redis, a vector database, steady inference traffic | [See Optimized Cloud Compute](https://bit.ly/VuLtr) |
| Cloud GPU, A100 80GB | 80GB HBM2e, PCIe or SXM, partitionable into up to 7 MIG slices | Per-card hourly rate | Hourly | Single-node fine-tuning, multi-tenant inference, QLoRA on a 70B model | [See the A100 80GB configurations](https://www.vultr.com/products/cloud-gpu/nvidia-a100?ref=9738262-9J) |
| Cloud GPU, H100 | 80GB HBM3, FP8 precision and the Transformer Engine | Per-card hourly rate | Hourly | Large-model training and latency-sensitive inference | [See the H100 specifications](https://www.vultr.com/products/cloud-gpu/nvidia-h100?ref=9738262-9J) |
| Vultr Clusters | Multi-node GPU clusters on InfiniBand, provisioned by console or API, schedulable with Slurm or Kubernetes | Per-node hourly rate | Hourly, deploy and tear down at will | Jobs that need more than eight cards | [Plan a GPU cluster](https://bit.ly/VuLtr) |
| Bare Metal | Single-tenant dedicated servers, with GPU clusters available on this tier as well | about $120/mo* | Monthly | Sustained load, compliance requirements, data residency | [See Bare Metal](https://bit.ly/VuLtr) |

\*Entry prices for the non-GPU lines come from G2's Vultr pricing summary and are indicative; Vultr's own price list is the authority.

Two details in that table do more work than the rest.

The first is MIG on the A100. Vultr supports partitioning an A100 80GB into up to seven isolated instances, each with dedicated memory and compute. If you are serving several small models, or several customers, one card can replace several rentals, and each slice runs without contending with the others.

The second is Clusters. Multi-node GPU clusters connected by InfiniBand, provisioned through the console or the API, scheduled with Slurm or Kubernetes, available on both Cloud GPU and Bare Metal, with no reservation or special request. This is the gap that matters against Paperspace, whose documented multi-GPU option stops at a single 8-card machine.

Compliance is covered too: Vultr states its platform meets ISO and SOC 2+ standards, which is the sort of thing that decides a vendor for a regulated team before price gets discussed.

## Five setups, five different answers

**A student with no budget.** Kaggle Notebooks is the strongest free option: a P100 or two T4s, 12-hour sessions, 20GB of auto-saved output. Availability is limited and you can wait in a queue at busy times. Paperspace's Free plan does the same job if you want a persistent notebook rather than a 12-hour session. Neither is worth migrating away from, and neither justifies renting anything, because the paid options start at roughly $0.17/hr and a student running three experiments a month will spend more on the migration than on the compute.

**One card, fine-tuning a 7B to 13B model at 40 hours a month.** This is where the arithmetic decides. Forty hours on Paperspace's A100-80G is $127.20. The same forty hours runs $71.60 on RunPod's Secure tier at $1.79/hr, $79.60 on JarvisLabs at $1.99/hr, and $43.60 at Nodus's $1.090/hr. The gap between $127 and $72 dwarfs the $39 plan fee, which tells you what to optimise: the card rate, not the subscription. Vultr fits here if you want the A100-80G in PCIe or SXM form with the option to slice it later.
👉 [Look at Vultr's A100 80GB configurations](https://www.vultr.com/products/cloud-gpu/nvidia-a100?ref=9738262-9J)

**Multi-node training.** Paperspace's ceiling is an H100 x8 at $47.60/hr, and 80 H100 GPU-hours on that machine costs $476 at list price. Anything that needs more than eight cards, or a scheduler that treats nodes as a pool, has to go elsewhere. Vultr Clusters on InfiniBand with Slurm or Kubernetes is the on-demand answer without a reservation request, and Paperspace's three-region footprint is worth weighing if your dataset has to stay in a particular jurisdiction.
👉 [Price an H100 cluster on Vultr](https://www.vultr.com/products/cloud-gpu/nvidia-h100?ref=9738262-9J)

**Inference behind an API.** Modal is the clean fit for function-first Python work that fires on a cron, webhook or HTTP call and scales to zero between requests; it is not built for a persistent service or a full stack. Baseten handles autoscaling inference with optimised kernels, model chaining and Prometheus-compatible metrics, but it offers no bring-your-own-cloud and no place to put Postgres or Redis, so the rest of the stack lives somewhere else. RunPod's serverless endpoints bill per second and skip egress fees, which is a good match for bursty traffic. Vultr's angle here is MIG slices plus a Cloud Compute box for the API layer, in one account and one bill.

**A team that needs private networking, RBAC and audit logs.** Northflank is the one product in this group that does BYOC, container-level isolation, built-in CI/CD, job scheduling and a database in the same project, with role-based access control and cost attribution on top. If those controls are a requirement rather than a nice-to-have, that combination is the reason to look, and it is also the reason not to: you are buying a platform, not a card. Vultr covers the adjacent case with single-tenant Bare Metal, Kubernetes Engine and managed databases in the same account.

## The rest of the field, at verified prices

| Platform | Published rate (checked November 11, 2026) | Where it's the better call | What to watch |
| --- | --- | --- | --- |
| RunPod | A100 80GB $1.79/hr Secure, from $1.39/hr Community; H100 $3.99/hr Secure, from $2.69/hr Community | Per-second billing, no egress fees, one-click templates for common models | Community tier trades price for host guarantees |
| Lambda | V100 $0.79/hr, A100 40GB $1.99/hr | Zero data transfer fees and fast interconnects for training | US-only regions, no serverless, no orchestration |
| Vast.ai | From $0.16/hr; RTX 3090 under $0.20/hr; A100 spot from $0.67/hr | Interruptible work where price beats reliability | No SLA, unprivileged Docker isolation, checkpoint every 30 to 60 minutes |
| JarvisLabs | A100 80GB from $1.99/hr, H100 from $2.49/hr | Closest like-for-like replacement for a Gradient notebook: Jupyter and VS Code on every instance, per-minute billing, pausable | Smaller platform, so peak GPU availability can dry up |
| Nodus | A100 80GB $1.090/hr, H100 SXM $2.600/hr, A6000 $0.333/hr | Cheapest verified per-hour A100 in this group, with checkpoint restore onto a replacement machine | Prepaid credit only, no free tier, and these are the vendor's own rates |
| Kaggle | Free P100 or 2×T4, 12-hour sessions | Zero-cost experimentation | Queue at peak times, 20GB output cap, unsuited to anything above a small model |
| Colab Pro+ | $49.99/month | Priority H100 access on a flat monthly fee | Built for short experiments, not sustained training |

One caution about list prices: they move. Every number above is dated, and a provider that looks 40% cheaper today can reprice a card without notice.

## Three things that catch people leaving Paperspace

**Free machines are public on the Free plan.** The 6-hour auto-shutdown and the single-notebook limit get the attention, but public visibility is the constraint that should change behaviour. No proprietary data, no API keys, no client datasets. If that rules the free tier out for you, the plan you were on was never the cheap option you thought it was.

**Marketplace and spot capacity needs a checkpoint habit.** Vast.ai runs without an SLA, isolates tenants with unprivileged Docker containers unless you pick a verified host or a single-tenant option, and instances have been reported dying mid-run. Write model, optimiser and step files to persistent storage every 30 to 60 minutes. The same discipline applies anywhere you rent interruptible capacity, and it is the difference between losing an hour and losing a week.

**A subscription can cost more than the hours it unlocks.** The break-even points above are the whole argument: 46.2 hours a month on the Pro plan, 35.8 on Growth against an A100 at $1.090/hr, 117.1 against an A6000 at $0.333/hr. Those figures use Nodus's rates, so recalculate with whichever provider you are actually considering. If your monthly hours land under the line, drop the plan.

## Picking one

Match the setup to the platform rather than the platform to the brand:

- Your hours fit the free machines and the data isn't sensitive, so **stay on Paperspace Free or Pro**.
- One card, 20 to 60 hours a month, no managed notebook needed, so pick the **cheapest verified hourly rate with no plan**, and compare RunPod, JarvisLabs and Nodus against each other.
- GPU plus the API, database and queue in one account, with a preference for multi-node later, so **price Vultr**.
- More than eight cards or a scheduler across nodes, so go to **Vultr Clusters** or a platform built for distributed training.
- Production inference with autoscaling and nothing else to host, so **Modal, Baseten or RunPod serverless**.
- BYOC, RBAC and audit logs as hard requirements, so **Northflank**.

Three things to confirm before you enter a card number anywhere: the billing granularity (per second, per minute and per hour produce different bills for the same workload), whether the card you want is actually available in a region close enough for your latency budget, and whether egress is metered. The first two decide the total; the third decides whether the total you calculated is the total you pay.

👉 [Check Vultr's current plans, GPU configurations and rates](https://bit.ly/VuLtr)
