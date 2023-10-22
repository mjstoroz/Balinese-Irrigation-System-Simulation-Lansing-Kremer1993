# Model Description

This is a model description of a replication of the model described in Lansing (1991) and Lansing and Kremer (1993) in Netlogo. The model presented in Lansing and Kremer (1993) consists of two kinds of analysis. One analysis is the evaluation of the performance of the Bali irrigation system at different levels of coordination, the other analysis is a hill climbing model where synchronizing cropping pattern emerge at the level of temples like the empirical data. Only the latter version of the model is presented here.

The model description follows the ODD protocol for describing individual- and agentbased (Grimm et al. 2006) and consists of seven elements. The first  three elements provide an overview, the fourth element explains general concepts underlying the model’s design, and the remaining three elements provide details. Additionally, details of the software implementation are presented.

## Purpose

The purpose is to show that by only local interactions between subaks, groups of farmers, emergent patterns of cropping schedules emerge at the whole irrigation system with high production levels. This illustrate that managing a large scale irrigation system does not require top down control.

State variables and scales The model consists of 172 subaks, who act as independent agents. These subaks are within a complex irrigation network that describes 2 rivers. A total of 11 dams act as points where subaks get their water or return their water left over.

Subaks are indirectly connected via waterways, they share water from the same dams, and directly via spatial proximity which makes it possible for pests to spread between neighboring subaks. Each subak has a specific area of land available which affect the demand for water. Process overview and scheduling One time step is equivalent to one month. Subaks decide each year which of the 21 cropping patterns to follow. A cropping pattern determines which crop to plant in which month. There are 3 rice varieties, and there is fallow and vegetables.

Subaks imitate the cropping pattern of a neighbor, if there is a neighboring subak who had a higher harvest per ha during the previous year. Each subak has a number of subaks defined as their neighbors. Lansing and Kremer (1993) are not clear in their definition of neighbors, but correspondence with Kremer suggests that the pest related subaks are considered the neighbors, those are directly connected in the spread of pests. Practically, a subak can not directly implement a new cropping pattern in the new year, since it may still have crops on the fields. It is not clear how Lansing and Kremer implemented this. To reproduce their results, we assumed that each year the pest start at initial values (0.01) and a new cropping pattern. If we don’t initially the pest level, the system gets locked into low harvest rates.

The monthly schedule of activities is to determine for all subaks the following processes:

- Demand for water
- Water flows
- Rice
- Pest
- Harvest
  
## Design concepts

**Emergence**: the evolving pattern of cropping plans mimic the temple groups at the masceti temple level. Thus local adjustments of synchronization of cropping plans lead to high performance of harvest with similar organization structures as observed in the field.

**Adaptation**: Each year the subak can adapt their cropping plan.

**Fitness**: harvest of rice per ha is used to evaluate the performance of a cropping plan for a
subak.

**Stochasticity**: the only stochasticity is the initialization of the cropping plans.

## Initialization

Each subaks get randomly allocated one of the 21 cropping plans, and start this plan in a randomly determined month.

## Input

The following data are input in the model, and are provided in the code of the model and the text files that are imported during the initialization process:

- Water network of dams and subaks
- Network of subaks who can disperse pest to eachother
- rainfall per month for different elevations (three different scenarios are provided)
- cropping plans.
- area of subaks
- Masceti temple subaks belong to
- for each crop: maximum yield, duration of crop on land before harvest, sensitivity to pests, growth rate of pests when specific crop in on the land.

## Submodels

### Demand for irrigation water

Demand for irrigation water for a subak depends on the difference between the water the crop needs per ha and the rain that fell per ha. This difference is multiplied by the area of land available in the subak.

Demand = (cropuse – rain)*area

### Water flows

Starting with dams upstream, the waterflows in dams and subaks are calculated taking into account rainfall and water streaming into canals from upstream.

### Rice

If not enough water is derived for rice, there is waterstress. If rice takes X months to grow, each month the rice is assumed to grow 1/X part. If only a fraction Y<1 of the demanded water is provided, the rice grows that month for a smaller part: Y/X

### Pest

For all neighboring subaks between which pests can disperse calculate the sum of pests level of the own subak minus the pest level of a neighboring subak (sumpestdif). Then calculate dc

dc = pestdispersalrate * sumpestdif * (dt / dx)

where dt/dx is 0.3 and dc is the change in pest biomass that is dispersed. Note that dc can be negative if more pest is moved to other subaks than are imported. and finally determine the pest level: 

Pests = growthrate *(Pests + 0.5*dc)) + (0.5 * dc)

See more discussion on this equation in Janssen (2006)

### Harvest

If it is time to harvest, the harvest of a subak is calculated as follows: 

harvest = ricestage * yldmax * (1 – pests * pestsens) * area,

where ricestage is a value between 0 and 1 representing which fraction of the water demand is provided over the course of the time rice was on the land, yldmax is the maximum yield in optimal conditions, and pestsens is the amount of rice lost for a unit of pest on the land.

### Model implementation

Based on the code received from James Kremer in an unknown language for ancient Macintosh machines. This code includes the basic structure and data of the Bali irrigation system, not this specific hill climbing version. Janssen (2006) adapted this version with a more general imitation rule. The model is implemented in NetLogo, using version 5.0.4. The model mimics the general pattern of harvest amounts per ha as presented in Lansing and Kremer.

![Alt text](image-1.png)

Figure 1: A. Typical run of Net Logo implementation leading to harvest per ha of around 7.5. B. Results of the Lansing and Kremer (1993) publication.

The average harvest level is somewhat lower than the original publication (see Figure 1). Since there are a few details of the model implementation not clear, this is not surprising, but the main patterns are similar.

The current model implementation might be improved in the following ways:

- Using links instead of sets for implementing irrigation network in Net Logo 4.0 (I had originally implemented it in Net Logo 3.1 which did not include links)
- More comments in code on the functioning of the model.
- Figuring out what causes the lower yields in Net Logo version compared to original model

## References

Janssen (2006)
Lansing and Kremer (1993)
Grimm et al. (2006)
Lansing (1991)
