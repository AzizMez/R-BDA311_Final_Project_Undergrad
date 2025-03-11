BDA-311 Final Project
================
Georgio Ghnatios, Aziz Mezraani
Fall 2023

## GitHub Documents

This is an R Markdown format used for publishing markdown documents to
GitHub. When you click the **Knit** button all R code chunks are run and
a markdown file (.md) suitable for publishing to GitHub is generated.

# Abstract

As music enthusiasts, we wanted to create an innovation that improves
our community by understanding user groups. We identified three key
frustrations:

1.  Unorganized learning material scattered across the internet.
2.  Lack of a strong community.
3.  Difficulty in sharing and publishing musical work.

Using a five-step design process, we employed various tools and
techniques, including: - **R** for clustering and NLP, - **Persona
maps** to understand our user group, - **Ideaboardz** for
brainstorming, - **Figma** for prototyping, - **Maze** for usability
testing.

The final product is **MusicCamp**, a learning platform that offers
structured courses, career tracks, and a community-driven music-sharing
experience.

# Empathize Step

## Data Collection

A structured **Google Form** survey gathered data on demographics, music
learning, and engagement. Shared across social media, we collected **106
responses** within three days.

## Data Analysis (Cluster Analysis in R)

We processed the data in **R**, converting categorical variables to
numerical values for analysis. Using **k-means clustering** (elbow
method, k=4), we identified four user groups:

``` r
# Load necessary libraries
library(tidyverse)
library(cluster)
library(factoextra)
library(dendextend)
library(RColorBrewer)

# Read and scale data
music_data <- read.csv("C:/Users/azizm/OneDrive/Documents/BDA-311-Final-Project/Relation to Music Responses Processed_Augmented.csv")
clusterData <- scale(music_data)

# Determine optimal number of clusters
fviz_nbclust(clusterData, kmeans, method = "wss") + geom_vline(xintercept = 4, linetype = 2)

# Perform k-means clustering
k4 <- kmeans(clusterData, centers = 4, nstart = 100)
fviz_cluster(k4, data = clusterData)
```

Heatmaps helped interpret cluster characteristics:

``` r
# Create heatmap for visualization
sub_grp <- k4$cluster
clusterData <- data.frame(clusterData, Cluster = sub_grp)
clustAverages <- clusterData %>% group_by(Cluster) %>% summarize(across(everything(), mean, na.rm = TRUE))
center_reshape <- clustAverages %>% pivot_longer(cols = -Cluster, names_to = "features", values_to = "values")

# Generate heatmap
hm.palette <- colorRampPalette(rev(brewer.pal(11, 'RdYlBu')))(100)
ggplot(center_reshape, aes(x = features, y = Cluster, fill = values)) +
  geom_tile() +
  scale_fill_gradientn(colors = hm.palette) +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

# Define Step

## Interviews

We conducted **six interviews** to understand user struggles. Key
takeaways: - Difficulty balancing music with studies/work. - Overwhelmed
by scattered learning resources. - Struggles with sharing compositions
and finding an audience.

## Latent Dirichlet Allocation (LDA) in R

We refined LDA analysis to extract three key themes:

``` r
# Load LDA package
library(topicmodels)

# Prepare data for LDA
lda_data <- read.csv("C:/Users/azizm/OneDrive/Documents/BDA-311-Final-Project/SurveyRawData.csv")
dtm <- DocumentTermMatrix(VCorpus(VectorSource(lda_data$response_text)))
lda_model <- LDA(dtm, k = 3, control = list(seed = 1234))
lda_terms <- terms(lda_model, 5)
print(lda_terms)
```

Key themes: 1. **Community needs** – Users seek a music-loving community
for support. 2. **Learning struggles** – Need structured, accessible
resources. 3. **Sharing barriers** – Uncertainty on how to publish music
work.

# Ideate Step

We transformed user needs into **“How Might We”** questions and
brainstormed solutions on **Ideaboardz**. After a structured voting
process, we selected the best idea:

### **Final Idea: A Community-Based Music Learning Platform**

- A website offering **guided courses**, **career tracks**, and
  **practice sessions**.
- An **XP-based leaderboard** to encourage engagement.
- A **community feed** inspired by Reddit.
- A **publish section** for sharing compositions and hosting live
  concerts.

# Prototype Step

We created **low-fidelity** and **high-fidelity** wireframes in
**Figma**, focusing on: - Home page - Career tracks - Learning courses -
Community feed - Music sharing (Publish section)

[Prototype
Link](https://www.figma.com/proto/zSVk8WSATAyhjqDm4E2vsM/Untitled)

# Test Step

Using **Maze**, we tested the prototype with **38 respondents**. Key
insights: - UI is user-friendly, but **some buttons were misleading**. -
Users enjoyed the **community** and **learning features**. - Suggestions
included **direct messaging, AI assistance, live events, and
competitions**.

### **Planned Improvements**

- Add **functional buttons** to reduce misclicks.
- Implement **AI-based recommendations**.
- Introduce **live events and competitions**.
- Enable **direct messaging** for users to connect.

------------------------------------------------------------------------

# Conclusion

Our **MusicCamp** platform aims to create a **structured**,
**community-driven** learning experience for music enthusiasts. With
further iterations, we plan to refine the prototype, integrate user
feedback, and develop the final product.

Thank you for an amazing semester! ❤️🎵
