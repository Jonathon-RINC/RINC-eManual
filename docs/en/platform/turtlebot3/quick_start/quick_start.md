---
layout: archive
lang: en
ref: quick-start
read_time: true
share: true
author_profile: false
permalink: /docs/en/platform/turtlebot3/quick-start/
tabs: "ROS"
tab_title3: Noetic
tab_title6: Humble
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 4
---

<style>body {counter-reset: h1 2 !important;}</style>
<div style="counter-reset: h2 0"></div>

{::options parse_block_html="true" /}

**Only ROS1 Noetic and ROS2 Humble are officially supported by ROBOTIS at this time. However, development of TurtleBot features for other ROS distributions is ongoing. The following chart provides an overview of the features supported by each ROS Distribution**

✓ : Available  <br>
? : Unverified<br>
X : Unavailable<br>

|        Features        | Kinetic | Melodic | Noetic | Dashing | Foxy | Galactic | Humble |
|:----------------------:|:-------:|:-------:|:------:|:-------:|:----:|:--------:|:------:|
|         Teleop         |    ✓    |    ✓    |   ✓    |    ✓    |  ✓   |    ✓     |   ✓    |
|          SLAM          |    ✓    |    ✓    |   ✓    |    ✓    |  ✓   |    ✓     |   ✓    |
|       Navigation       |    ✓    |    ✓    |   ✓    |    ✓    |  ✓   |    ✓     |   ✓    |
|       Simulation       |    ✓    |    ✓    |   ✓    |    ✓    |  ✓   |    ✓     |   ✓    |
|      Manipulation      |    ✓    |    ✓    |   ✓    |    ✓    |  ✓   |    ?     |   ✓    |
| Home Service Challenge |    ✓    |    ✓    |   ✓    |    X    |  X   |    X     |   X    |
|   Autonomous Driving   |    ✓    |    X    |   ✓    |    X    |  X   |    X     |   X    |
|    Machine Learning    |    ✓    |    ✓    |   X    |    ✓    |  X   |    X     |   X    |

|       Examples       | Kinetic | Melodic | Noetic | Dashing | Foxy | Galactic | Humble |
|:--------------------:|:-------:|:-------:|:------:|:-------:|:----:|:--------:|:------:|
| Interactive Markers  |    ✓    |    X    |   X    |    X    |  X   |    X     |   X    |
|  Obstacle Detection  |    ✓    |    X    |   X    |    ✓    |  X   |    X     |   X    |
|   Position Control   |    ✓    |    X    |   X    |    ✓    |  X   |    X     |   X    |
|   Point Operation    |    ✓    |    X    |   X    |    ✓    |  X   |    X     |   X    |
|        Patrol        |    ✓    |    X    |   X    |    ✓    |  X   |    X     |   X    |
|       Follower       |    ✓    |    X    |   X    |    X    |  X   |    X     |   X    |
|       Panorama       |    ✓    |    X    |   X    |    X    |  X   |    X     |   X    |
|     Auto Parking     |    ✓    |    X    |   X    |    ✓    |  X   |    X     |   X    |
| Auto Parking (Vision)|    ✓    |    X    |   X    |    X    |  X   |    X     |   X    |
|   Multi TurtleBot3   |    ✓    |    X    |   X    |    X    |  X   |    X     |   X    |

<section data-id="{{ page.tab_title3 }}" class="tab_contents">
{% include en/platform/turtlebot3/quickstart_noetic.md %}
</section>

<!-- <!-- <section data-id="{{ page.tab_title1 }}" class="tab_contents">
{% include en/platform/turtlebot3/quickstart_kinetic.md %}
</section> -->

<!-- <section data-id="{{ page.tab_title2 }}" class="tab_contents">
{% include en/platform/turtlebot3/quickstart_melodic.md %}
</section> -->

<!-- <section data-id="{{ page.tab_title4 }}" class="tab_contents">
{% include en/platform/turtlebot3/quickstart_dashing.md %}
</section> -->

<!-- <section data-id="{{ page.tab_title5 }}" class="tab_contents">
{% include en/platform/turtlebot3/quickstart_foxy.md %}
</section> -->

<section data-id="{{ page.tab_title6 }}" class="tab_contents">
{% include en/platform/turtlebot3/quickstart_humble.md %}
</section>

<!-- <section data-id="{{ page.tab_title7 }}" class="tab_contents">
{% include en/platform/turtlebot3/quickstart_windows.md %}
</section> -->

<!--

Log:
20201018
- JS code is addeds to default.html.
- The made js code performs adding a class named "selected" to .archive class.
- when archive class name is changed, I want the include specific fragnments will appear and the other fragments not show up (display: none;)

20201019
- If statement only works one time when the pate is loaded. Manipulate css property, display: none or block.

20201020
- {::options parse_block_html="true" /} 옵션을 통해, Block Level 의 블럭과 마크다운을 같이 사용할수있다.
- {: .} 로 통해, ID 또는 class 지정이 가능하다.

20201029

- Page 로드시, Object에 data를 저장하여 Tab이 게속 선택되어지게끔 해야한다.
- tutorialrepublic.com/faq/how-to-keep-the-current-tab-active-on-page-reload-in-bootstrap.php#:~:text=Answer%3A%20Use%20the%20HTML5%20localStorage,tab%20selected%20on%20page%20reload.

-->
