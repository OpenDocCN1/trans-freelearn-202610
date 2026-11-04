# Technical requirements

All the topics covered in this chapter are accessible to Startup and Educational Fusion 360 license types. If you have a Commercial Fusion 360 license, in order to conduct a shape optimization study, you will also need access to the Simulation extension (https://www.autodesk.com/products/Fusion-360/simulation-extension), or you can use Autodesk Flex tokens to solve shape optimization studies (https://www.autodesk.com/benefits/flex).

Even though we will not be conducting a generative design study in this chapter, if you have a Commercial Fusion 360 license and would like to conduct a generative design study, you will need access to the Generative Design extension (https://www.autodesk.com/products/Fusion-360/generative-design-extension), or you can use Autodesk Flex tokens to solve generative design studies (https://www.autodesk.com/buying/flex?term=1-YEAR&tab=flex).

Autodesk offers a 14-day trial for both the Simulation extension and the Generative Design extension for all Commercial users. You can activate your trial within the Extensions dialog of Fusion 360.

Neither Automated Modeling, Generative Design, nor any of the simulations mentioned previously (Shape Optimization and Linear Static Stress) are available for Personal/hobby licenses of Fusion 360.

The lesson files for this chapter can be found here: https://github.com/PacktPublishing/3D-Printing-with-Fusion-360.

# Getting Started with Automated modeling

In previous chapters, we covered how to use parametric modeling and direct modeling in order to make changes to existing designs. Using solid and mesh modeling techniques, we created holes, pockets, and hollowed-out parts, which are all useful methods for lightweighting parts.

The Automated Modeling functionality within the Design workspace of Fusion 360 is a great tool for designers to create organic-looking lightweight components. In this section, we will focus on creating new models using this generative technique. After choosing a design alternative, we will modify the volume of the outcome. Later, we will manually edit the organic shape to further lightweight the design.

Automated modeling is available within the DESIGN workspace's SOLID tab's AUTOMATE panel. Using automated modeling, users can generate design alternatives between geometries that they want to connect. This function requires a minimum of two faces to be selected as input faces. It also allows users to select zones where a model should not be created. When using this command, Fusion 360 will generate six design alternatives. It will then allow you to explore the design that best suits your needs. Three of the design alternatives are created using smooth connections between the selected faces. The remaining three are created using sharp connections. Now, let us explore this functionality with an example dataset and walk through the iterative process a designer may follow while working on a project.