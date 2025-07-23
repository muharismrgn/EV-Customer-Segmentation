# Eletric Vechiles (EV) Customer Clustering

# Context
In a data-rich environment such as a transportation service involving motorcycle rentals or tracking, understanding user behavior is essential for improving operational strategies, optimizing service allocation, and enhancing customer satisfaction.

The dataset in this project consists of user profiles, trip data, and motorcycle vehicle data. By leveraging RFM (Recency, Frequency, Monetary) modeling and clustering techniques, we aim to segment users based on their behavioral patterns.

# Stakeholders
- **Business and Operations Team**: Seeks to understand usage patterns to allocate motorcycles effectively and reduce operational costs.

- **Marketing Team**: Interested in identifying high-value or loyal users for targeted promotions or retention efforts.

- **Product & Strategy Team**: Aims to personalize features or incentives based on user segments.

# Problem Statement
How can we effectively segment motorcycle service users (riders) based on their behavioral patterns using RFM modeling and clustering techniques, in order to support personalized service strategies and targeted marketing?

We aim to create data-driven user segments and analyze their behavioral profiles to support decision-making across key business functions

# Goals
**User Segmentation**: Group riders into distinct segments using RFM (Recency, Frequency, Monetary) analysis and clustering algorithms.

**Behavioral Insights**: Analyze and interpret the behavioral traits or characteristics of each rider segment to derive actionable insights.

# Analytical Approaches
1. Data Preprocessing & Feature Engineering
    - Merge user profile, trip data, and vehicle data.
    - Clean missing values, handle anomalies, and engineer RFM features:
        - Recency: Time since the last trip.
        - Frequency: Number of trips per user.
        - Monetary: Total trip duration or value-based proxy.

2. Exploratory Data Analysis (EDA) and Dashboard Reporting
    Before modeling, we conducted comprehensive EDA to understand distributions, patterns, and relationships across datasets. We built an interactive dashboard using Looker Studio, which visualizes:
    - Rider distribution and frequency
    - Trip intensity by location and time
    - Behavioral trends across users
    [Here for Dashboard and Report Link](https://lookerstudio.google.com/s/tOgdouyA5Ks)

3. Segmentation Modeling
    - Scale features using StandardScaler.
    - Determine optimal number of clusters using:
        - Elbow Method 
        - Silhouette Score
    - Apply clustering algorithms:
        - K-Means Clustering
        - Agglomerative Clustering
    - Visualize clusters:
        - 2D pairplots and 3D scatter plots
        - Dendrogram for hierarchical clustering interpretation

4. Cluster Analysis
    - Summarize average RFM scores per cluster.
    - Interpret each segment's characteristics (e.g., loyal users, inactive users, heavy users)

# Metric Evaluation
To assess the quality and validity of user segmentation, we use:
- **Silhouette Scor**e: Measures cohesion and separation between clusters.
- **Elbow Method**: Determines the optimal number of clusters by analyzing inertia.
- **Dendrogram**: Visual tool to support interpretation of hierarchical clustering structure and distance between users/groups.


# Results

| Model | Elbow Cluster | Silhouette Score | Training Time(s) | 
|-|-|-|-|
| K-Means Clustering | 3 | 0.719 | 0.066 | 
| Agglomerative Clustering | 3 | 0.704 | 0.001 | 

Both K-Means and Agglomerative Clustering methods, along with evaluation metrics such as the Elbow Method, Silhouette Score, and Dendrogram Analysis, consistently identify three optimal rider segments. This agreement across methods indicates a stable and well-defined clustering structure in the data, allowing for confident interpretation and strategy development.

- **`Power Riders Cluster`**:
    
    - Characteristics:

        - Low recency: These riders have used the platform very recently, indicating high engagement.

        - High frequency: They take trips frequently and consistently over time.

        - High monetary: They tend to spend more time using the platform or take longer trips, signaling heavy usage.

        - These riders likely have subscribed passengers or loyal customers who rely heavily on platform.

    - **`Insight & Strategi`**:

        - Introduce a tiered loyalty program offering benefits such as discounted services, priority access, or bonus points for consistent use.

        - Provide scheduled maintenance packages or alerts to ensure their vehicles remain in good condition due to high usage.

        - Send targeted educational content (e.g., on vehicle care or safety) to reduce downtime due to mechanical issues.

        - Maintain high service quality and minimize disruptions to keep satisfaction levels high.

- **`Active Riders Cluster`**:  
    - Characteristics:

        - Moderate recency: They have used the platform relatively recently, though not as recently as Power Riders.

        - Moderate frequency: Their trip activity is steady but not intensive.
        
        - Moderate usage value: Their overall usage is balanced, indicating potential to increase or decrease based on platform experience and incentives.


    - **`Insight & Strategi`**:

        - Implement targeted promotions, such as time-limited offers bonus point, to encourage more frequent use.

        - Targeted promotions, reminders, or content recommendations

        - Monitor behavior to detect shifts, whether toward higher engagement or signs of disengagement and respond proactively.


- **`Inactive Riders Cluster`**:
    - Characteristics:

        - High recency: These users haven't used the platform for a long time, indicating disengagement.

        - Low frequency: Very few trips logged.

        - Low trip value: Their overall platform use is minimal.


    - **`Insight & Strategi`**:

        - These users are at risk of churn, Investigate if their vehicle is inactive due to damage, disuse, or lack of interest.

        - Send reactivation offers or surveys to understand their situation.

        - Make loyalty point bonus and reminders to bring them back

        - Offer a big discount for checking the vehicle if there really have a problem