---
layout: page
title: Pong in an FPGA with VHDL
description: Project to implement the pong game in a Terasic FPGA through VHDL
img: assets/img/pong_fpga.png
importance: 12
category: work
---

During my last year of the Bachelor's I had one course fully dedicated to understand and learn to program on an FPGA. The language that was used for this purpose was VHDL
and the board that we used was the FPGA Max 10 DE10-Lite. This board contains an Altera MAX 10 FPGA.

<p align="center">
  <img width="400" height="300" src="/assets/img/fpga_max_10_layout.jpeg">
</p>
<div class="caption">
    FPGA Max 10 DE10-Lite board - Terasic Technologies (mouser)
</div>

As you can see the board contains not only the FPGA, but also many peripherals to implement real applications and practice. Some of them can also be identified in this connection diagram:

<p align="center">
  <img width="500" height="300" src="/assets/img/terasic_block_diagram.webp">
</p>
<div class="caption">
    10 DE10-Lite board - Terasic Technologies - Diagram (mouser)
</div>

We used the Intel Quartus Prime as the software editor and synthesizer to program the FPGA. The course was very practic and after many lab lessons, we had to develop a final project. For this project I decided to
implement the pong game in the FPGA so that I can get also familiar with one of the peripherals, the VGA interface. In a few words, this game contains a VGA driver for the FPGA an image generator and a score shown in 7-segment
displays. Many challenges that were fun to solve where the VGA driver where I got familiar on how an image is generated through this kind of interface and screens and also modelling the behaviour of the ball, collisions and how it bounces. The following video shows the game:

<p align="center">
    <iframe width="600" height="400"
        src="https://youtube.com/embed/hZkz6tCa-Tc"
        title="YouTube video player"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
        allowfullscreen>
    </iframe>
</p>
<div class="caption">
Presetation of the pong game in the FPGA
</div>

And this is the code of the project:

{% if site.data.repositories.github_repos %}
<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for repo in site.data.repositories.github_repos %}
    {% include repository/repo.html repository=repo %}
  {% endfor %}
</div>
{% endif %}
