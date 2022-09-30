---
layout:     post
title:     "Smart Sensing-02"
subtitle:   "以智能水表为例"
date:       2021-10-14
author:     "Yvan"
header-img: "img/bg/bg4.jpg"
header-mask: 0.3
header-img-credit: "Hands off my tags! Michael Gaida from Pixabay"
header-img-credit-href:  "pixabay.com/de/users/michaelgaida-652234/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1696481"
no-catalog: false
mathjax: true
category: "auto"
tags:
    - 传感器
    - 测量
typora-root-url: ..
typora-copy-images-to: ..\img\in-post\smartsensor
---



> 基于 **Prof. Andrea Cominola** 参与的论文[1\][3\][4\]

## Water Metering

**Basic Comparison**

- traditional Water meter
  - low-frequency reading
  - coarse resolution m³
  - Different types:
    - Mechanical flow meters
    - Magnetic flow meters
    - Ultrasonic flow meters: 管道外测量
- smart meter
  - data resolution up to 72 pulses/L
  - data sampling frequency and logging 5-10s

**Benifits of Smart Water Meter [2]**

- automate meter reading
- improved demand and revenue forecasting
- establish leak alerting
- establish a detailed customer data portal
- offer monthly billing
- establish detailed water balancing
- establish a capability for data analytics
- increase knowledge of coustomers and assets

### WDMS water demand management strategies

Benefits and challenges of using smart meters for advancing residential water demand modeling and management [4]

<img src="/img/in-post/smartsensor/image-20211013154907172.png" alt="image-20211013154907172" style="zoom: 67%;" />

### **challenges in use**

- Hardware VS Software potential/limitation
  - HW: 长期稳定的设备 等
  - SW: analysis data, commuication ...
- Big Data management
- Privacy and intrusiveness
- Cybersecurity

### Implications of sampling frequency on water applications

Implications of data sampling resolution on water use simulation, end-use disaggregation, and demand management [1]

#### **Sampling Frequency**

10s, 1min, 5min, 15min.1h, 1d ...

#### **Implications**

- **Water end-use  disaggregation**

  - One Measure → Many End-Uses (Toilet/Shower/Garden/...) 一个预测多分类任务

  - **指标**：

    - individual appliance usage

    - individual appliance pattern

      ![image-20211013153617238](/img/in-post/smartsensor/image-20211013153617238.png)

- **Leakage detection** 

  - Leakage 种类

    <img src="/img/in-post/smartsensor/image-20211013145644624.png" alt="image-20211013145644624" style="zoom:50%;" />

  - **指标**：

    - Average Water Loss

      ![image-20211013153531190](/img/in-post/smartsensor/image-20211013153531190.png)

- **Peak demand  estimation**

  -  Measuring frequency

    <img src="https://www.researchgate.net/profile/Andrea-Cominola/publication/322941029/figure/fig2/AS:726515956326407@1550226310984/Time-series-of-total-community-water-use-500-households-for-one-day-sampled-at-temporal.png" alt="Time series of total community water use (500 households) for one day sampled at temporal resolutions ranging from 10 seconds to a day. Daily pattern is characterized by two peaks, at approximately 8 am and 7pm." style="zoom: 67%;" />

  - **指标**：

    - Peak Estimation Error

    - Peak Estimation Time Gap

      ![image-20211013153759670](/img/in-post/smartsensor/image-20211013153759670.png)

    

- **Cost** 

  - **指标**：
    - Data Storage ( MB/(Household * Year) )

- **Feasibility**

  - **指标**：
    - Commercial availability

#### **Data**

- generation using [STREaM](https://github.com/acominola/STREaM)

- 5 Indoor fixtures were used (toilet, washmachine, shower, dish washer, faucet)
- ![image-20211013151259784](/img/in-post/smartsensor/image-20211013151259784.png)
- **Summary**
  - Data sampled with < 5 min resolution seem to be:  
  - Beneficial for different purposes (leak detection, peak estimation, end use  disaggregation)  
  - Not easy to get with commercial products

## A multi-utility vision Water+Electricity

水消耗和电力消耗的联系：城市和个人

Integrated intelligent water-energy metering systems and informatics [3]







---

[1] Abdallah, Adel & Cominola, Andrea & Giuliani, Matteo. (2018). [Implications of data sampling resolution on water use simulation, end-use disaggregation, and demand management.](https://www.researchgate.net/publication/322941029_Implications_of_data_sampling_resolution_on_water_use_simulation_end-use_disaggregation_and_demand_management) Environmental Modelling and Software. 102. 10.1016/j.envsoft.2017.11.022. 

[2] Monks, Ian, Rodney A. Stewart, Oz Sahin, and Robert Keller. (2019). [Revealing Unreported Benefits of Digital Water Metering: Literature Review and Expert Opinions](https://www.mdpi.com/2073-4441/11/4/838) *Water* 11, no. 4: 838.

[3] Stewart, Rodney & Nguyen, Khoi & Beal, Cara & Zhang, Hong & Sahin, Oz & Bertone, Edoardo & Vieira, Abel & Castelletti, Andrea & Cominola, Andrea & Giuliani, Matteo & Giurco, Damien & Blumenstein, Michael & Turner, Andrea & Liu, Ariane & Kenway, Steven & Savic, Dragan & Makropoulos, Christos & Kossieris, Panagiotis. (2018). [Integrated intelligent water-energy metering systems and informatics: Visioning a digital multi-utility service provider](https://www.sciencedirect.com/science/article/pii/S1364815217311271). Environmental Modelling and Software. 105. 10.1016/j.envsoft.2018.03.006. 

[4] Cominola, Andrea & Giuliani, Matteo & Piga, Dario & Castelletti, Andrea & Rizzoli, Andrea-Emilio. (2015). Benefits and challenges of using smart meters for advancing residential water demand modeling and management: A review. Environmental Modelling & Software. 72. 198 - 214. 10.1016/j.envsoft.2015.07.012. 

