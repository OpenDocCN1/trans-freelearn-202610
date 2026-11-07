
## Learn
## Blender Simulations
the Right Way

Transform your artistic vision into stunning, realistic simulations with Blender 4.0

![](img/2bdfac576180ebfe5ee8960a6eec7f99_0_0.png)

Stephen Pearson

## Learn Blender Simulations
the Right Way

Transform your artistic vision into stunning, realistic simulations with Blender 4.0

Stephen Pearson

![](img/2bdfac576180ebfe5ee8960a6eec7f99_1_0.png)

## Learn Blender Simulations the Right Way
Second Edition

Copyright © 2025 Packt Publishing

*All rights reserved.* No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

**Portfolio Director:** Rohit Rajkumar
**Relationship Lead:** Neha Pande
**Content Engineer:** Anuradha Joglekar
**Program Manager:** Sandip Tadge
**Growth Lead:** Namita Velgekar
**Technical Editor:** Tejas Mhasvekar
**Copy Editor:** Safis Editing
**Proofreader:** Anuradha Joglekar
**Indexer:** Tejal Soni
**Production Designer:** Ponraj Dhandapani
**Marketing Owner:** Nivedita Pande

First edition: November 2022
Second edition: April 2025

Production reference: 1070425

Published by Packt Publishing Ltd.
Grosvenor House
11 St Paul's Square
Birmingham
B3 1RB, UK

ISBN 978-1-83620-005-5

www.packtpub.com

To my parents, who helped me start on this journey of learning 3D. And to my wife, Jamie, for her encouragement and inspiration as we both write and create together.

- Stephen Pearson

## Contributors

## About the author

**Stephen Pearson** is a 3D artist, graphic and product designer, video producer, and the founder of BlenderMadeEasy. He has worked in the 3D industry for over 11 years and has produced several best-selling courses on architectural visualization, **Visual Effects (VFX)** simulations, and animation. He started using Blender in 2014 as a fun hobby, but now he uses it to teach other 3D artists around the world. In his spare time, he enjoys exploring local breweries, going for a ride on his motorcycle, and playing with his two dogs.

*I would like to first thank my family, friends, and loving wife for all the encouraging words and support throughout the process of writing this book. I would also like to thank all the Blender developers for making such an amazing program free and open source for anyone to try. And, of course, thank you to all the people who have watched my tutorials and supported me throughout the years.*

## About the reviewers

**Mariselvam M** is a VFX artist who works on visual effects, animation, and movie projects that demand technical precision and artistic creativity. Their passion for the field was sparked by a love for cartoons and movies, which continues to inspire their work today. Known for their expertise in Blender and Houdini, Mari is passionate about crafting visually compelling narratives In addition to their professional endeavors, Mari enjoys traveling with friends, drawing creative inspiration from new experiences.

> *I extend my heartfelt thanks to my parents and friends for their unwavering support.*

**Thirunavukkarasu M** is an AR/VR Developer and 3D Artist with a strong focus on creating immersive experiences using Unity, Blender, and Meta Quest devices. Currently, he is working at the Logistics Sector Skill Council, developing cutting-edge AR/VR solutions to enhance training and visualization for the logistics and supply chain industry.

In his current role, he designs and implements interactive simulations and 3D assets that improve learning outcomes and operational efficiency. His expertise spans Unity development, 3D modeling, and mixed reality (MR) applications, with a proven track record of delivering projects like **Caddy VR** for industrial visualization and **Fighter Jet Training Modules** for defense preparedness. His work is driven by a commitment to innovation and a desire to push the boundaries of what's possible in extended reality (XR).

## Join our community on Discord

Join our community's Discord space for discussions with the author and other readers:
https://packt.link/learn-blender-simulations-discord-invite

![](img/2bdfac576180ebfe5ee8960a6eec7f99_6_0.png)

## Table of Contents

Preface xiii

## Part 1: Using Mantaflow for Fire, Smoke, and Fluids

1

## An Introduction to Mantaflow 3

- Technical requirements 4
- What is Mantaflow? 4
- Gas and liquid simulations 4
- Gas simulations 5
- Liquid simulations 8
- What you need to create a simulation 9
- Domain objects 9
- Flow objects 10
- Effectors 11
- Creating your first simulation 11
- Adding the objects 12
- Creating the simulation 13
- Setting up the camera 17
- Rendering the animation 18
- Setting the output 19
- Summary 21

2

## Understanding Domains, Flows, and Effectors 23

- Technical requirements 23
- Domains 24
- Settings 25
- Cache 28
- Gas domains 29
- Liquid domains 36
- Diffusion 40
- Particles 41
- Mesh 44
- Guides 47
- Collections 51
- Field Weights 51
Viewport Display 52
Flow objects 54
Render 53
Effectors 60
Summary 62

3

## Creating a Realistic Campfire Simulation 63

Technical requirements 63
Creating sparks using a particle system 74
Creating the campfire simulation 64
Creating the particle material 77
Setting up the domain 64
Adding the final details 78
Setting up the inflows 66
Summary 80
Adding effectors 69
Exercise 81
Creating the fire materials 70

4

## Creating a Waterfall Using Mantaflow 83

Technical requirements 83
Creating materials 95
Setting up the simulation 84
Adding vector blur 96
Adding inflow and effectors 89
Rendering animations 101
Adding foam particles 91
Summary 105

5

## Creating a Realistic Explosion 107

Technical requirements 108
Rendering the explosion in Cycles and EEVEE 121
Creating the particle system 108
Rendering in Cycles 121
Creating the simulation 113
Rendering in EEVEE 123
Setting up the domain 113
Summary 125
Adding the inflows 115
Creating the material 117
Creating the smoke 117

## Part 2: Simulating Physics with Soft Bodies and Cloth

6

## Getting Started with Soft Bodies 129

Technical requirements 129
Introduction to soft body simulations 130
Understanding collision objects 132
Understanding soft body settings and values 135
Object settings 136
Cache 137
Soft Body Goal 140
Edges 143
Self Collision 148
Solver 150
Field Weights 151
Interacting with multiple soft bodies 152
Summary 153

7

## Creating a Soft Body Obstacle Course 155

Technical requirements 155
Creating the simulation 156
Adding the soft body 156
Adding collisions 157
Creating the cloth simulation 161
Using Soft Body Goal 164
Creating a glass material in EEVEE 169
Setting up camera tracking 173
Summary 176

8

## An Introduction to Cloth Simulations 177

Technical requirements 177
What is a cloth simulation? 178
Understanding collision objects 180
Understanding cloth simulation settings and values 182
Physical Properties 184
Cache 191
Shape 192
Collisions 196
Property Weights 198
Field Weights 200
Summary 201

9

## Creating a Realistic Flag 203

Technical requirements 204
Making the materials 216
Adding the objects 204
Creating the flag material 217
Creating the flag 204
Creating the hook material 219
Creating the flagpole 206
Creating the flagpole material 220
Creating the rope 209
Rendering the animation 220
Creating the hooks 210
Summary 222
Creating the simulation 212

## Part 3: Diving into Rigid Bodies

10

## An Introduction to Rigid Bodies 225

Technical requirements 225
Dynamics 235
What is a rigid body simulation? 226
Rigid Body World 236
Understanding the Rigid Body settings 228
Rigid Body Object Menu 239
Settings 228
Using rigid body constraints 240
Collisions 230
Constraint settings and types 241
Summary 249

11

## Creating a Rigid Body Physics Course 251

Technical requirements 251
Collapsing the cubes 265
Creating the simulation 252
Adding the camera animation 268
Simulating the wrecking ball 252
Setting up the render 269
Creating the domino effect 254
Summary 271
Simulating the elevator 255
