==================
Custom Units
==================

The units library defines 1023 special custom units.  These are custom units intended to specify a specific type of unit which doesn't have a normal unit base definition.  The key idea behind the custom  units is that they can be multiplied, divided by some normal powers of distance, mass, or time units and can be inverted

In strings these can be represented by "CXUN[X]"  Where X is some number between 0 and 1023.

In C++ code they can be generated by

.. code-block:: c++

   precise_unit new_cxc_unit=generate_custom_unit(code);


A set of checks and queries is available to check for custom_units.

-  bool precise::custom::is_custom_unit(detail::unit_data udata);
-  bool precise::custom::is_custom_unit_inverted(detail::unit_data udata);
-  unsigned short precise::custom::custom_unit_number(detail::unit_data udata);

These checks will operate regardless of any m/kg/s unit combination or inverted units.

Custom units in Use
----------------------------
there are a few custom count units in use for specific clinical units Many of these units defy conversion to other known units but are used in pharmacological contexts
So there is no translation to other units and cannot be converted except to multiple of the same unit.  There are often well established tests for these units but no good way to convert them to other units.  Many of these units come from `UCUM <https://unitsofmeasure.org/ucum.html>`_.

-   custom_unit(37):  is `hounsfield units <https://radiopaedia.org/articles/hounsfield-unit?lang=us>`_ used it CT and radiology
-   many units in UCUM are defined like `[MPL'U]` or `[mclg'U]`  for this context they define some unit which doesn't interact with other units in any known fashion.  The notion used in the units library for string translations is that these define custom units.  Rather than individually define the library takes a hash of the part of the unit coming before the `'U]'` and generates a 10 bit hash.  That 10 bit hash is used as the custom code for the units.

The other custom units are available for use or the one with known definition can be use if there is no domain conflicts.

The primary usage of these is for units that are procedurally defined and often used in the context of per mass or per volume or per time.

Implementation Details
------------------------
Custom units use a combination of nearly all the different fields in the unit_base class, with the exception of count and radians.  Based on the definitions used the custom units can be taken as per length/area/volume/mass/second with no issues.  Some of the unit fields are used for defining an index and others are used purely for identification purposes.
