---
layout: archive
lang: en
ref: simulation
read_time: true
share: true
author_profile: false
permalink: /docs/en/platform/turtlebot3/nav_simulation/
tabs: "ROS"
tab_title1: Humble
tab_title2: Jazzy
tab_title3: Noetic
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 14
---

<style>body {counter-reset: h1 6 !important;}</style>
<div style="counter-reset: h2 2"></div>

<!--[dummy Header 1]>
  <h1 id="dummy">Simulation</h1>
  <h2 id="dummy">Navigation Simulation</h2>
  <p class="dummy_content">TurtleBot3 Navigation Package</p>
<![end dummy Header 1]-->

## [Navigation Simulation](#navigation-simulation)

Just like with SLAM in the Gazebo simulator, you can select or create various environments and robot models in the virtual Navigation world. However, a complete map has to be prepared before running Navigation. Other than the preparation of a simulation environment instead of bringing up the robot, Navigation Simulation is pretty similar to that of real-world TurtleBot3 [Navigation][navigation].  

{::options parse_block_html="true" /}

<section data-id="{{ page.tab_title1 }}" class="tab_contents">
{% include en/platform/turtlebot3/simulation/nav_simulation_humble.md %}
</section>

<section data-id="{{ page.tab_title2 }}" class="tab_contents">
{% include en/platform/turtlebot3/simulation/nav_simulation_jazzy.md %}
</section>

<section data-id="{{ page.tab_title3 }}" class="tab_contents">
{% include en/platform/turtlebot3/simulation/nav_simulation_noetic.md %}
</section>
