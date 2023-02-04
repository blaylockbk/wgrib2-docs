# wgrib2: questions

Question: I use -small_grib to make a smaller domain. Why is the "smaller"
file almost 3x larger?

Answer: -small_grib creates a new grib message. The default compression is "off".
You can use c3 (complex number 3) compression by "-set_grib_type c3". BTW
this is why grib_filter on the nomads.ncep.noaa.gov server can create
larger files when making a regional subset.

Question: Why does wgrib2 create smaller complex-packed files
than cnvgrib?

Answer: Complex packing can be done in different ways. wgrib2 uses
a slower algorithm than cnvgrib and makes a smaller file.

Question: I get a warning about the decode may differ from the WMO standard, why?

Answer: Some software/libraries may differ from the WMO standard when encoding and decoding a constant field.
The warning is based on the center and testing of software/libraries developed by that center.
The testing was done many years ago and may no longer be appropriate. If any center
feels that the warning is inaccurate, let me know and I will remove the warning.

The common bug is that when encoding a constant field, the decimal scaling
factor modifies the field reference value. Some codes ignore the decimal scaling which
was the case in the grib1 standard. To turn off
the wgrib2 warning, make sure the decimal scale factor is zero when the field is constant.

|

|     |
| --- |

|

---

|
| [NOAA/](https://www.noaa.gov/)
[National Weather Service](https://www.nws.noaa.gov/)
[National Centers for Environmental Prediction](https://www.ncep.noaa.gov/)
Climate Prediction Center
5830 University Research Court
College Park, Maryland 20740
[Climate Prediction Center Web Team](/comment-form.html)
Page last modified: Dec 21, 2015
| [Disclaimer](https://weather.gov/disclaimer.php) | [Privacy Policy](https://weather.gov/privacy.php) |

|

---

> Description: questions

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/usage_questions.html>_
