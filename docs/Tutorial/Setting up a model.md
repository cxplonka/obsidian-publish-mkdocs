For the modeller, each compilation of a density/susceptibility model always consists of two activities that result from a theoretical approach: a **body** must be defined which contains a mass/magnetic material (here density and/or susceptibility), and the distances from stations where the gravity and/or magnetic fields were measured. Therefore, model stations must be defined (see below).

At first, the origin point of the model (its zero point) has to be fixed, which is not only the origin of model geometry, but also of model stations, of the corresponding gravity/magnetic fields which should be matched, of the voxel cube and of any available additional map information, for example from a geographical and/or a digitized geological map.

We start with the explanation on how to handle the bodies and continue with the explanation for stations.

Please use the [[Introduction]]

---

### [](#21-model-bodies)2.1 Model bodies

In **IGMAS+**, density in space can be defined either in terms of

1.  triangulated polyhedra surrounding a certain volume of constant density ([Fig. 2.1](#figure-two_polyhedra_model)) or
2.  a 3D **voxel cube** ([Fig. 2.2](#figure-voxel_model)) containing numerous voxels, each carrying its own density value.

{{< figure library="true" src="two_polyhedra_model.png" lightbox="true" title="**FIGURE 2.1:** A model with two polyhedra of constant densities (pink and red)." numbered="false" id="two_polyhedra_model" >}}

{{< figure library="true" src="voxel_model.png" lightbox="true" title="**FIGURE 2.2:** A voxel model defining sedimentary density structures." numbered="false" id="voxel_model" >}}

All polyhedra with the same density definition make up a model **body** (Body Manager), while a single model body may be divided into several geometrically separated polyhedra (called "**indexed body parts**"). The hull of a polyhedron is composed of interfaces. The geometry of each interface is defined by vertices.

The user-defined positioning of these **vertices** is crucial for the triangulation process by which **IGMAS+** geometrically approximates the 3D density structure. The obtained model topology is defined based on the position of such vertices on pre-defined, parallel oriented, vertical planes (vertical **working sections**) ([Fig. 2.3](#figure-model_construction)).

{{< figure library="true" src="model_construction.png" lightbox="true" title="**FIGURE 2.3:** Illustration of basic terms for **IGMAS+** model construction: vertices (black points), triangles (black lines), and working sections (vertical planes)." numbered="false" id="model_construction" >}}

These working sections are the virtual scenes for implementing any interactive modifications of model geometries, for example, by **adding**, **deleting**, or **moving vertices**.

In case the model is built on **working sections** ([Fig. 2.3](#figure-model_construction)), there are no vertices located between the working sections. The user therefore should define the number, spacing and horizontal orientation (strike direction) of these working sections

1.  to keep sufficient control of model geometries throughout the interactive modelling process and
2.  to allow for a proper analysis of elongated gravity anomalies (i.e., orient working sections perpendicular to the strike of major anomalies).

Working sections are always parallel ([Fig. 2.4](#figure-section_geometry)).

{{< figure library="true" src="section_geometry.png" lightbox="true" title="**FIGURE 2.4:** Four steps that are important when defining the geometry of the model bodies by “sections”. (a) Decide which anomalies should be modelled. (b) Select the area to be modelled. (c) Mark the sections on the area to be modelled and define the intersections of the sections with the anomaly. (d) Consider which of the sections and the intersections are really important to model the anomaly correctly." numbered="false" id="section_geometry" >}}

{{% callout note %}} **Remember:** The simpler the model - the better it is. {{% /callout %}}

**IGMAS+** calculates gravity effect either for a flat or a spherically curved model. The latter is required for very extensive model domains. To get an impression of the Earth surface "depression" of a spherical model compared to a flat one, note the following numbers:

In addition, it should be considered that in a spherical model the calculation of the direction of the vertical component changes continuously according to the curvature of the Earth because it always points to the centre of the Earth. Thereby, a spherical model assumes that the Earth is approximated by a sphere; an elliptical shape cannot yet be realized (but this is negligible for many lithosphere modelling applications). Test calculations have shown that for a model extending by, e.g., $2000 \times 2000$ km$^2$ and reaching a depth of $200$ km, there is a difference in calculated gravity between a spherical and flat modelling approach of about $20--25\times 10^{-5}~~$m/s$^2~~$($20--25$ mGal) .

Depending on the user’s objectives and the characteristics of gravity/magnetics independent information at hand, there are basically three different ways of building up an initial 3D density model ready to be analysed in terms of its gravity/magnetics effects:

-   a) “Defining sections” approach: define working sections before loading or creating model vertices.
-   b) “Loading layers/interfaces/horizons” approach: load point sets forming body interfaces before defining working sections.
-   c) “Loading a voxel cube” after defining the model space according to (a) or (b)

When selecting the **“sections” approach (a)**, the user builds the model from scratch by first defining the 3D model extent and the vertical (working) sections and then loading or creating vertices to construct interfaces that separate bodies of different density. Any gravity-independent data which are loaded into the obtained model space to help constructing the density bodies (e.g., bitmaps, point sets) can be projected and visualised on the vertical sections. In this case, the sections should be appropriately positioned to keep the projection-related distortions of observed structures small. Since all vertical sections need to be parallel, it might not be possible to represent all available structural information ideally in the model domain. Hence, option (a) suits well to solve generic problems independent of correctly georeferenced structures. Additionally, the approach may be selected if the spatial coverage of gravity-independent structural input data (e.g. seismic profiles, boreholes) is very limited with respect to a larger number of observed gravity anomalies. For example, the interpreted structure of a single 2D seismic section could thus be continued laterally by inferring a variety of consistent 3D models from the observed gravity field. Thereby, one would strategically start with a simple density configuration and increase the model extent and complexity stepwise.

If the spatial coverage of structural input data is dense enough and the depth/thickness configuration of several density bodies can be derived directly and modelled outside of **IGMAS+**, then the **“layer” approach (b)** offers the proper functionality. In this case, the user can load continuous **interfaces** or **horizons** -- sets of regularly or irregularly spaced points with XYZ coordinates. Each horizon defines the top of a spatial domain that -- according to gravity-/magnetics-independent observations -- has been identified as a potential contrast in density with respect to adjacent domains. The loaded interfaces/horizons are stacked by IGMAS+ and collectively define the default 3D model extent (to be changed optionally), while the working sections are defined only after this stage. As part of the model building process, **IGMAS+** interpolates between the loaded XYZ points of a horizon, derives the intersections of the horizon with the working sections and accordingly creates a number of vertices positioned on the latter. Hence, the initially loaded point sets are not identical with the final vertices representing a horizon! Care should be taken that the **horizons**/**layers**/**interfaces** do not intersect each other (corresponding to a negative thickness of the respective layer in between).

Generally speaking, there are no specific suggestions for the use of case (a) or (b). Recently, there has been some preference for the use of case (b), since many modelers prefer to work with predefined layers from open databases (for example [CRUST1.0](https://meetingorganizer.copernicus.org/EGU2012/EGU2012-3743-1.pdf), [LITHO1.0](https://doi.org/10.1002/2013JB010626), [ETOPO1](http://dx.doi.org/10.7289/V5C8276M) and many others). Layers cannot be eliminated from the model afterwards. But you can assign the same density to the horizon as to its neighbouring horizons, to the side of it, above it or below it. Then it has no more gravity effect on the model stations.

After setting up a model either following approach a) or b), one or several model bodies may be selected for **“voxelization” approach (c)**, i.e., for being differentiated into numerous voxels, each carrying its own density value. In this way, smaller-scale density variations derived from independent observations (e.g., seismic, mineralogical-petrological) can be superimposed on the geological structure and considered for the gravity calculation. Each voxel is associated with an effective density representing the sum of:

1.  a constant body density and
2.  a voxel density.

One way of defining the voxel density is to first create a voxel grid and then apply a certain function to implement physical laws or empirical concepts such as seismic-velocity-to-density conversions or depth-controlled porosity-density functions. Alternatively, the voxel cube may be defined completely by data import including both the coordinates and density values of the voxels. Additional **IGMAS+** functionality related to voxel cubes is provided in terms of:

1.  multiplication of the voxel density with a voxel factor for fast model modifications,
2.  automatic edge effect minimization and
3.  transformation of voxel-related density variations into triangulated isosurfaces.

Note that currently only one voxel cube can be applied to an **IGMAS+** model.

---

### [](#22-model-densities)2.2 Model densities

Concerning the **density value** assigned to each model body, it does not matter if absolute or relative density values are chosen, since the resulting gravity effect only depends on the density differences at modelled interfaces and the distances from stations. This implies that gravity modelling alone cannot determine absolute values of densities. For a deeper understanding of these statements, a more detailed description of the basic mathematical-physical formulas for the calculation of the gravity effect in **IGMAS+** would be beneficial. However, this would go beyond the scope of this tutorial. Therefore, the original publications by [Götze and Lahmeyer (1988)](/igmas/igmas-pages/-/tree/devel/content/publication/goetze-1988) or by [Götze (2014)](/igmas/igmas-pages/-/tree/devel/content/publication/goetze-2014) are recommended.

---

### [](#23-model-stations)2.3 Model stations

The observed and calculated gravity fields are defined at the stations, each being defined by its $XYZ$ coordinates. The station height ($Z$) refers to its elevation with respect to the geoid, in case of elliptical coordinates -- also to the ellipsoid. It either represents the vertical position of the original gravimeter measurement (daylight surface for terrestrial data, height above the sea level for ship data, flight height for airborne and satellite data, etc.) or a reference level to which the acquired remote data have been continued (see [chapter 4](/igmas/igmas-pages/-/tree/devel/content/tutorial/4_fitting_gravity) and [Fig. 4.1](/igmas/igmas-pages/-/tree/devel/content/tutorial/4_fitting_gravity#figure-cba_simple), [Fig. 4.2](/igmas/igmas-pages/-/tree/devel/content/tutorial/4_fitting_gravity#figure-fa_simple) and [Fig. 4.3](/igmas/igmas-pages/-/tree/devel/content/tutorial/4_fitting_gravity#figure-fa_difficult)). The only condition that must be met for the height of all stations is that they must be located above all model masses. It has its meaning for modelling exclusively when using Free Air anomalies.

_Practical experience and examples for heights_ of the user-defined reference level ([Fig. 4.1](/igmas/igmas-pages/-/tree/devel/content/tutorial/4_fitting_gravity#figure-cba_simple), [Fig. 4.2](/igmas/igmas-pages/-/tree/devel/content/tutorial/4_fitting_gravity#figure-fa_simple) and [Fig. 4.3](/igmas/igmas-pages/-/tree/devel/content/tutorial/4_fitting_gravity#figure-fa_difficult)) can be found here:

Note that to avoid numerical/theoretical problems (namely, local outliers in the calculated gravity), the stations should neither be located inside a model body nor precisely on its edges or surfaces (hence, check the $Z$ values with respect to the top of the uppermost model body). In case of doubt, add $13$ cm to the height of the body surface -- the height of the gravimeter measuring system (see [Fig. 4.2](/igmas/igmas-pages/-/tree/devel/content/tutorial/4_fitting_gravity#figure-fa_simple)).

The spatial coverage and spacing of the stations (i.e. their $XY$ coordinates), together with the wavelengths of the associated observed anomalies, provide limits to the scales and resolution of subsurface density heterogeneities that can be derived -- an important aspect when planning a gravity modelling project ([Fig. 2.4(b,c,d)](#figure-section_geometry)). In the context of resolution, **IGMAS+** allows gravity calculations for a large number of stations -- but user should keep in mind the memory size limit (refer to the {{% staticref "files/IGMAS_User_Manual.pdf" %}}**IGMAS+ User Manual**{{% /staticref %}}, chapter 2.1.2).

In general, there are two different types of station data that **IGMAS+** users may use: irregularly or regularly spaced data. When working with original irregularly spaced measurements (i.e. scattered and clustered $XY$ station coordinates), the observed and the modelled gravity have the same relative position with respect to the causative density bodies. If one chooses to interpret anomalies that have been transferred to a regular grid (e.g., by use of ICGEM data sets, [chapter 4.6](/igmas/igmas-pages/-/tree/devel/content/tutorial/4_fitting_gravity#46-remarks-on-the-use-of-icgem-gravity-datasets), or other grids), however, one accepts that the model bodies have a smaller effect with respect to the stations than the real bodies due to interpolation procedures. Hence, it is generally recommended to use irregularly distributed stations instead of gridded gravity data.