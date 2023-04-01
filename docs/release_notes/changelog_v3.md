# Changelog - Version 3 series

# v3.1.1 (April 14, 2022)

removed Unix_time_old.c -unix_time_old, this was used for testing, forgot to remove

function.sh: export LC_ALL=C prior to sort, so order is consistent at all locations M Schwarb

updated NCEP grib table 4/26/2022. locally defined SOILM and SOIL_M added, M Schwarb

updated get_code_Table_4.10.sh CodeTable_4.10.dat, CodeTable_3.20.dat, M Schwarb

updated get_code_Table_3.15.sh BUFRTable_0_01_007.dat, M Schwarb

updated BUFRTable_0_02_019.dat BUFRTable_0_02_020.dat CodeTable_3.2.dat, M Schwarb

ieee_pk.c error message: "ieee_pk: grib2 data section is limited to 4G bytes"
changed to "ieee_pk: grib2 data section is limited to 4G-1 bytes"
fixed test for too large, old: assumed that int was 32 bits

mk_bms(..): bms_size calculation could overflow for 4G-8 < ndata < 4G

complex_pk.c: fixes for 4G grids

unpk_complex.c: 4G fix, number bits would overflow, change n -> n_bytes, n_bits

get_hex: 1st byte was printed as %d instead of %.2u, reported by R. Dabrowski, thanks RD

Set_byte.c change: else seclen = uint4(sec[i[); by else seclen = sec[i] ? uint4(sec[i]) : 0;

rd_bitstream_flt: n is now unsigned int (4G)

rd_bitstream: n is now unsigned int (4G)

add_many_bitstream: n is now unsigned int (4G)

complex_pk.c: free(itmp2)

Unmerge_fcst.c: 4G fix

Undefined. extern int nx*, ny*; -> extern unsigned int nx*, ny*;

New*grid.c extern int nx*, ny*; -> extern unsigned int nx*, ny\_;

New_grid.c extern unsigned int nx, ny; -> extern int nx, ny;

-import_lonlat: old: fseek to start of file, not 4G
new: fseek to start of file if EOF, removed debug that was not 4G
new: check to see that 0 was read in

Fcst_ave.c: 4G fixes

-ilat: 4G

rdieee_file(): fixed error if ndata/4 > 2G and using header

aec_pk.c: ECMWF found that buffer was too small in the case that the data had high entropy
increase buffer size by 5% + 256, Thanks S. Najm

makefile: add HAS_CMAKE, more libraries will need cmake, libaec and openjpeg for now

makefile: libaec updated to version 1.0.6

unpk_0.c: improve openmp parallelization of calls to rd_bitstream

unpk_complex.c openmp parallelization of calls to rd_bitstream used by unpk_0.c

ijsmall_grib and small_grib works for rotated lat-lon grids

-rpn, parallelized print_rms and print_corr

update OpenJPEG to v2.5.0

decenc_openjpeg.c, remove use of bpp

fix grib2/makefile: USE_PROJ4 didn't work, missing quote, bug report J. Assheton

removed warning count for equal areal Lambert

fixed problem with equal area Lambert, UK Met fcst, bug report J. Assheton
grb2.h old #define GDS_Lambert_Az_Lo1(gds) (uint4(gds+42) _ 0.000001)
new #define GDS_Lambert_Az_Lo1(gds) (int4(gds+42) _ 0.000001)

Proj4.c improve error messages

Gctpc.c remove warning_LamAZ .. no limit to warnings to aspherical equal area Lambert

Proj4.c: proj4_get_latlon() remove redundant mallocs

Proj4.c: abs -> fabs

Proj4.c: remove handling of GDT=0

makefile: always install Proj4

wgrib2.h: enable Proj4 as default

wgrib2.c, Config.c: changed priority of geolocation to :gctpc, Proj4, internal

Earth.c: axes_earth(sec, &major, &minor) => axes_earth(sec, &major, &minor, &is_spherical)
affects Earth.c Gctpc.c gctpc_ll2xy.c grid_ident.c New_grid.c Proj4.c proj4_initialize.c
spaceview.c spaceview2ij.c

Earth.c f_radius, radius now call axes_earth. f_radius output changed

geolocation set: RPN.c (sto_lat, sto_lon), Import_latlon.c read_latlon.c, grid_def.c

f_geolocation(..): added new values to -geolocation (external)

gctpc_get_latlon: new: will not work for aspherical eq area lambert, require proj4

OpenJPEG: \#define to $Hdefine in makefile for OpenJPEG, thanks J Stroik

enabled interpolation neighbor-budget for ip2lib_d

added "ncep grid core", "ncep grid t170" should produce the same results but
slightly different northern most Gaussian lat. This version produces the
same section 3 as coming from CORe using gcc v4.8.5

makefile: USE_NETCDF4, USE_HDF5DIR changed from 0/1 to 0/compile/system/INC:LIB
for using provided netcdf4/hdf5 libraries. Note external libraries are not tested,
however netcdf4/hdf5 are not tested because of compile problems. Note clean compile
is not the same as a working HDF5 library. contributed by M. Fontana .. thanks

Config.c: changes to show netcdf4/hdf5 libraries

Added JMA pdts .. testing by R. Hanai, thanks
Small_grib.c: fix free of non-allocated memory

simple_pk.c: fix for oneAPI, random seg fault with oneAPI but not other compilers
Could not see error with original code. Compiler bug?

Config.c: fixed order geolocation support for proj4, clarified default

updated lib/makefile to make shared library for new intel compiler (icx/ifx)

grib2_split: changed to shell script

Ftime2.c: convert seconds to minutes in X-Y sec (type) fcst

added -set_ftime_mode: added int ftime_mode, Set_ftime_mode, changed init.c Ftime2.c units.c
by request of NBM

removed -set_version_ftime, only way to get ftime1 and set_ftime1 is explictily

# v3.1.0 (October 7, 2021)

unpk_complex.c: better vectorization
The next two fixes improve the accuracy of the calculations. Mathematically the same
results but by not simplifying the calculation, the optimized version (-march=...) of the
code are getting the same results as the non-optimized version. The differences
are round-off errors but it is nice to get 0.0 vs 1e-15 (when values are O(1)).

unpk_complex.c: (ref_val+udata[i])*factor -> (ref_val0+udata[i]*factor_2)\*factor_10
for more accuracy when -march=znver2

unpk_0.c: (ref+flt[i])*factor -> (ref0+flt[i]*bin_scale)\*dec_scale
for more accuracy when -march=znver2

wgrib2 was failing in a MPMD program with a write error. Error message did not say name of
output file which made debug more difficult. 3 changes to add filename to error meessages.

wgrib2.h: struct seq_file: added char filename[STRING_SIZE], to store filename

rd_seq_file.c fopen_file: save filename in file->filename
added filename to error messages

wgrib2.c added filename to error messages

-ens_processing: closed output file on exit, minor cleanup

-ens_qc: new option faster than -ens_processing for ens mean and spread.
Mean and spread defined if N > 0. For -ens_processing, mean and spread
are defined only if N = numnber of ensemble members.

fix -unix_time: bad glib fails for UTC and daylight savings time
reset $TZ not working, year 2038 problem (overflow of 32-bit ints)
use get_unixtime(..) from the netcdf package

get_unixtime(..): moved from Netcdf_sup.c to get_unixtime.c, used by -netcdf and -unix_time

makefile: remove DISABLE_TIMEZONE because -unix_time is now C89 (ansi C)
which is the minimum specs for the required C compiler - best_scaled_value(..) fixed for exponent overfloa numbers> 10^127 and 10^-126
added checks for integer overflow : i = floor(val + 0.5)
hasn't been a problem because best_scaled_value(..) is used for vertical
levels, and levels don't have extreme values - -set_lev: problem if abs(numerical value of level) is > 2e9, problem or 2e7 mb
didn't handle byte <-> signed bytes correctly
int8.c: added int1_char(..), int -> signed byte
added scaled_char(..), (factor, value) -> p[0..4] (level)
cleanup flt2scaled(..), best_scaled_value(..)
fixed: set_lev(..), parse_level1(..)
Int_Power(..): converted int to unsigned int, so avoid a future compiler message
cleanup of fixed_surfaces(..)

-set_prob: cleanup, new use scaled_char(..)

added -set aerosol_size, -set aerosol_wavelength

-set_ftime2: support 2@1 hour min(13-14 hour acc fcst)++ and 2@1 hour min(13-14 hour acc fcst)--

set_ftime2: error if n >= N_MAX (complexity)

-ens: fixed ECMWF ensemble files, if code table 4.7 is undefined, set to 0 or 3, Manfred Schwarb

-number_of_coordinate_values_after_template: added, support on Code_Values.c

updated stat_proc_verf_time_location(unsigned char \*\*sec)

new pdt_len(..) replaces check_pdt_size(..)

check_pdt_size: remove calculation of expected pdt size, now calls pdt_len(..)

-hybrid: uses pdt_len(..) instead of prod_def_temp_size(sec);

-Sec4: calculation for free is wrong, change format to size and expected size
use pdt_len(..) instead of prod_def_temp_size(sec)

f_spectral_bands_extname(..) spectral_bands(..) removed prod_def_temp_size(..)
replaced by size of pdt (vs calculated size of pdt)

-set_pdt: cleaned up code, save vertical coordinates, replaced smallest_pdt_len(..) by pdt_len(..)
bug fixes

CodeTable.c: added code_table_4_16, more support for pdt 31, 32, 33,34, 35
added number_of_contributing_spectral_bands(sec) and location

-import_netcdf: new hypercube arg: \* can be used for start-N:count-N, cleanup
prints out hypercube data if npnts != ndata, include standard time

makefile changes for netcdf4: thanks David Ryglicki,

-set_radius: better help

-ftime2: remove code to check initial pdt size, do outside

-set_pdt: allow optional parmeters pdt:len:nc=1:n=1, new_pdt(...,char \*misc_arg)

-set_date: understands unix time -set_date u(integer)

-level: new: 57-58 generalized vertical height coordinate (150)

-set_level: new: 57-58 generalized vertical height coordinate (150)

-set_ftime2: change pdt 0->8 (old: -set_pdt +8:len new: -set_pdt +8:n=X) saves vert_cord data)

updated: zlib 1.2.11 -> 1.2.12, libpng 1.2.57 -> 1.2.59, netcdf-c-4.8.1.tar.gz, hdf5-1.12.1.tar.gz

-prob: unknown code table 4.9: old: "probtype=255 :" new: "prob ?? (code table 4.9=255)"
bug: sprintf(inv_out,"probtype=%d ") is missing inv_out += strlen(inv_out)

-set_ens_num: changed code to use new_pdt(..), promotes more pdts

size_all(..): tried parallelize .. slower because work per threads was too small
handle size of sections to 2G.

complex_grib_out(..): error if ndef < packing mode

changed -new_grid_wind -> -new_grid_winds New_grid.c fprintf(stderr,..) and Changes, M Schwarb

openmp_util.c: use reduction(max:) and reduction(min:) to reduce size of code (requires openmp v3.1)

# v3.0.2 (March 1, 2021)

-ndates, -ndate, -ndates_fmt: changed from misc option to setup option,

-ndates_fmt: better description

updated -ndates: write to inv_file rather than \*inv_out to avoid buffer overflows

fixed tar_all script to tarball one copy of each file, problem with BSD-varient tar - Reset_delayed_error.c: no output to inv_out - dec_png_clone(): Sec 5 octet 20 needs to match bit_depth from png decode.
A mismatch means that the grib file is not right.
With A mismatch, old wgrib2 will produce the wrong grid values for a png decode.
Now check for mismatch, and produce a delayed fatal error (with correct decode) - old png decode only handled bit depths of 8, 16, 24 and 32 correctly. Other bit_depths
would produce a silent error and give invalid grid values. - update png decode to handle valid png bit_depths, 1, 2, 4, 8, 16, 24 and 32. - dec_png_clone: change char *cout -> unsigned char *cout, added grib2_bit_depth - unpk.c: change char *c to unsigned char *c, added grib2_bit_depth to call to dec_png_clone() - wgrib2.c add DELAYED_ERROR_MISC, error must be explained in message to stdout - removed one of many warnings when compiling gctpc
fixed EOF.c comparison

gctpc: explicityly defined sign(x) to be int. sign(x) -> int gctpc_sign(double x)

makefile: changed lib/ to ${lib}

makefile: added ${mod} for location of fortran mods. Keep ${mod} == ${lib}

updated ncep grib table on 6/4/2021

Data.c f_stats(): code clean up, used reduction clauses (part of OpenMP 3.1)

int_min_max(): used reduction clause to cleanup code

Note: stat() is used, defined in IEEE Std 1003.1-2001. (POSIX-1)

added DISABLE_STAT, stat(..) is posix, used to test for pipes and disk files
if stat(..) does not exist, set DISABLE_STAT=1 in grib2/makefile
this results in flush_mode=1, and files are assumed to disk files
i.e. could try to random access or rewind on a pipe
updated ffopen.c, rd_seq_grib.c and grib2/makefile

remove unused code: delta_delta(), delta()

gauss2lat(): parallelized code

updated: -names description
fixed closest(): when staggered grid and latlon or spaceview
added gaussian2ij.c, updated closest(), closest_init()
note: for lon=90 and lon=-90, old code could give arbitray longitudes which is valid
note: for lat=0, could give either +/- lat which is valid

cleanup lat2ij.c, add public domain notice

-names: now understand NCEP, ECMWF, DWD (old: only ncep, ecmwf, dwd)

-config: if default WMO names was DWD, would display ECMWF.

Sec3.c orographic -> orthographic spelling (Earl Baker)

SPBRT spelling of temperature (Earl Baker)

Manfred Schwarb rewrote \*.sh scripts that read tables from NCO web pages,
new versions read data from WMO github Thanks!

updated get_code_Table_0.0.sh: CodeTable_0.0.dat: updated, text changes

updated get_code_Table_1.2.sh: CodeTable_1.2.dat: updated, text changes

updated get_code_Table_3.1.sh: CodeTable_3.1.dat: updated, text changes

updated get_code_Table_3.2.sh: CodeTable_3.2.dat: updated, text changes

updated get_code_Table_3.8.sh: CodeTable_3.8.dat: center -> centres

updated get_code_Table_3.11.sh: CodeTable_3.11.dat: updated, cleanup, text changes

updated get_code_Table_3.15.sh: CodeTable_3.15.dat: updated, text changes

updated get_code_Table_3.20.sh: CodeTable_3.20.dat: cap change

updated get_code_Table_3.21.sh: CodeTable_3.21.dat: text changes

updated get_code_Table_4.0.sh: CodeTable_4.0.dat: update, text changes

updated get_code_Table_4.3.sh: CodeTable_4.3.dat: update, text changes

updated get_code_Table_4.4.sh: CodeTable_4.4.dat: text changes

updated get_code_Table_4.4.sh: CodeTable_4.4.dat: text changes

updated get_code_Table_4.7.sh:

updated get_code_Table_4.9.sh: CodeTable_4.9.dat: text changes

new get_code_Table_4.10.sh: CodeTable_4.10.dat: comments lost

updated get_code_Table_4.11.sh: CodeTable_4.11.dat: text changes

updated get_code_Table_4.15.sh:

updated get_code_Table_4.91.sh: CodeTable_4.91.dat: text changes

updated get_code_Table_4.208.sh: CodeTable_4.208.dat: text changes

updated get_code_Table_4.212.sh: CodeTable_4.212.dat: text changes

updated get_code_Table_4.222.sh: CodeTable_4.222.dat: text changes, "Missing" added

updated get_code_Table_4.230.sh: codetable_4_230.c: updated, text changes

updated get_code_Table_4.233.sh: updates later

updated get_code_Table_4.240.sh: CodeTable_4.240.dat: text changes

updated get_code_Table_5.0.sh: CodeTable_5.0.dat: text changes

updated get_code_Table_5.1.sh: CodeTable_5.1.dat: cap changes

updated get_code_Table_6.0.sh: CodeTable_6.0.dat: text changes

CodeTable.c: update code for 4.233: use table from 4.230

ExtName.c: update code for 4.233: use table from 4.230

Code Table 4.233 has been removed

added is_uint(char \*p)

-set: chemical/codetable_4.230: fixed error if chemical starts with digit
cleanup, no error if applied to non-chemical template

-set: added aerosol/codetable_4.233

-misc: before :(chemical type): now :chemical=(chemical type): affects all inventories
needed for -set_metadata, esp when chemical had no name, now consistent with aerosol

-set_lev: if level == "" do nothing
if level == "no_level" and PDT has no level, do nothing

updated explaination for -process in Alias.c

update -set, added process

fix -import_grib, -import_grib_fs bug found by R. T. Inouye
error in NCEP-lib bug correction when main input record is a constant field and dscale != 0

init.c: nc4 = 0, mode changed to nc4 = 0 (netcdf3 mode)
nc4 needs to be conditional on netcdf (Dusan Jovic)

more -rpn is parallelized

for pdt with no fcst time (radar, satellite), -set_ftime2 will accept anl rather than error
affects pdt == 20 || pdt == 30 || pdt == 31 || pdt == 35 || pdt == 254

check_pdt_size: added pdt 254

-spectral_band: added sat=, ex "METEOSAT 10 SEVIRI 10.79 um" -> "sat=METEOSAT 10 SEVIRI 10.79 um"

-set: better error message

OpenJPEG update: v2.3.1 -> v2.4.0, multithreading is not enabled, update -config

updated wgrib2/makefile, fnlist.c, can be corrupted when make -jN when N > 1

Spectral_bands.c: remove misleading indentation, some minor cleanup

proj4_initialize.c: added projection->radius_major = r_maj initialization (from M. Schwarb, thanks)
note: code does not use projection->radius_major, so no change in the code behavior

Config.c: replace DWD by DWD1

grib2/makefile: new: allow USE_NAME=DWD, old: USE_NAME=DWD1

update: get_code_Table_4.222.sh (from M. Schwarb, thanks)

update: get_code_Table_4.10.sh (from M. Schwarb, thanks)

update: codetable_4_230.c (from M. Schwarb, thanks)

# v3.0.1 (February 24, 2021)

removed because wrong file went to ftp.cpc.noaa.gov

-netcdf doesn't output unique variable names for href file
using -set_ext_name 1 helps but some fields need ftime info to
uniquely identify the fields. So changed getExtName(), changed
int use_ext_name -> unsigned int type_ext_name.

change getExtName() to same parameters as getName(). The replacement
delim and space are now "global" variables and characters.

# v3.0.0 (September 2020)

unmerge_fcst: fix error messages

makefile fails for gnu make v4.3, fixed

makefile, lib/makefile: add make shared library support for clang/flang

makefile, lib/makefile: minor fixes, mac update for FTN_LEGACY, George Trojan

added -gribtable_used, thanks Manfred Schwarb

if USE_REGEX == 0, warn_nonzero_min_sec is never defined. reported Kyle Gerheiser, EMC
only affects compilers without the regex library (windows C compilers .. Watcom?)
split Match.c -> Match.c and Match_inv.c

updated for OpenJPEG: makefile, jpeg_pk.c unpk.c wgrib2.h Grib.c Config.c

added for OpenJPEG: enc_jpeg2000_openjpeg.c, decenc_openjpeg.c: from ECMWF with mods by Dusan Jovic

OpenJPEG: optional replacement for jasper

jpeg_pk.c: parallelize some loops

fixed: code table 4.8 was pointing wrong location

fixed: code table 4.7 had bad entries (200,201), updated code table 4.7

added more pdts to code table 4.8

fixed: ExtName.c and f_misc(..) with regards to trailing : in inventory .. should be the same inv

updated -set: cluster

added check for pdt size: check_pdt_size(sec),

added delayed fatal errors using last message > 1
problem, could not see last message that caused error, added check_pdt_size to delayed errors
redefined last_message: old: 0/1 (stop), new 0/1..N (stop), contains error reason for last_message > 0
updated wgrib2.c f_end(sec)

-bin: added check for error in writing header

added: -export_sec, -import_sec

added delayed error: local gribtable=255, grid size mismatch, ftime2 error

-limit: freed memory when finished

last_message: changed from int to unsigned int

makefile: initial support for nvc and nvfortran, fortran config update

Wind_uv.c: save->grib_type changed from int to enum output_grib_type

Cleanup CodeTable.c FlagTable.c

added ecmwf grib table, Manfred Schwarb wrote script to read ECMWF web pages

added -names (ecmwf|ncep), Names.c

changed: cname.c init.c, f_set_var()

fixed number of clusters in Clusters.c

removed debug print in f_code_table_4_233

fixed print in Submsg_uv.c

added check_pdt_size to wgrib2.h

cleaned code for eof_bin and eof_string, memory leak removed
added #include "fnlist.h" to Check_pdt_size.c

ffopen(): changed code to remove "note:" (code was good)

AAIG.c cleanup sprintf to remove warnings from gcc9

added -names dwd, updated cname.c Names.c gribtab.c, makefile

moved all gribtables to gribtables/(center)/

made directory grib2/wgrib2/gribtables/(dwd,ecmwf), move gribtables there

updated wgrib2/makefile to compile gribtab.c if gribtable.dat was changed

fatal_error.c combine all fatal_error\*(..) codes into one

fatal_error.c: removed #ifndef SIMPLE_ERROR because preprocessor symbol is unused

Time_processing.c: fixed error message, George Trojan

Gctpc.c: fixed error message, George Trojan

update Level.c

wgrib2(): test for too many cmd line arguments

upgraded ncep gribtable

added code_table_0_0_location

added code_table_1_0_location

fixed: -new_grid location, did not set discpline of GEOLAT/GEOLON (bug found by B. Reen)

minor change wgrib2/makefile

cleaned warnings for jpeg_pk.c/wgrib2.h when using OpenJPEG

changed isdigit(artN[i]) -> isdigit((unsigned char) argN[i]), Ndates.c Write_sec.c rd_inventory.c,
CubeFace2Global

changed isdigit((int) *p) -> isdigit((unsigned char) *p), Netcdf_sup.c

changed makefile, lib/makefile for cygwin_win, made lib/makefile shorter (shared library)

updated: c_api/\*.c to remove pointer warnings
