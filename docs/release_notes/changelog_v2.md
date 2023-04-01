# Changelog - Version 2 series

# v0.2.0.8   2/2019 - v2.0.8
        fix makefile to put png.h into include
        support for proposed cubed sphere (gdt-60)
        added new if-structure: else, elseif, endif, nested if
        added -start_timer -timer
        added -import_grib_fs
        -grib_out, checks if gds was changed, if ndata != npnts, warns and sets data to undefined
        added -wind_uv
        enhanced -wind_dir, wind_speed to have error messages like wind_uv
        -grid_def: recognize GEOLAT and GEOLON
        makefile: added for g95: -wFFLAGS+=-O2 -> wFFLAGS+=-c -O2
        -rpn: added sto_lat, sto_lon
        -wind_speed, wind_dir, diagnostics for no match if mode > 0
        added -set_gds, update_sec3
        -wind_dir: scaling .. to nearest degree
        complex_pk.c: parallelized a loop
        added int check_datecode(int year, int month, int day, int hour, int minute, int second);
        -ndate, -ndates now check for valid date code
        ip2lib_d: polefix for nearest neighbor
        added -new_grid grib (template) dummy new_file
            takes gdt from template, dummy will be used in irr2 files
        new_grid: wind_direction is now initialized by init(); affects calls to wgrib2
           require -new_grid_winds
        new_grid: added grib for ip2lib_d (IPOLATES==3)
        fixed debug statement in c_api/grb2_cmds();
        fixed mk_gdt, test for failed lookup
        added -new_grid location
        fixed: import_text
        updated code table 4.9, Prob.c CodeTable.c
        added: Level.c "highest level > xxx DBz"
        -set_prob can take values of "", so can make set_meta work.
        set_meta.c: fix prob
        -new_grid, undefined -new_grid_winds, fix error message
        same_sec4_but_ensemble(), do not check code table 4.6
        -ens_processing:  can ensemble process perturbed runs and control runs
                    code table 4.6 is not checked
        -submsg_uv: added flush_file()
        new_grid: support for FLANG (LLVM compiler)
        added -new_grid_order, replaces -submsg_uv
        more support for pdt 70
        -GRIB: free memory when mode==-2
        -ieee: free memory when mode==-2
        -bin: free memory when mode==-2
        -grib_out: free memory when mode==-2
        added string2time_unit()
        set_date: calls string2time_unit()
        time_processing: calls string2time_unit()
        ndates: calls string2time_unit()
        updated gribtable: STPRATE -> STRPRATE (Stratiform Precipitation Rate:kg/m^2/s)
                 BNEGLAY -> BNEGELAY:Bourgoiun Negative Energy Layer
                 TSEC -> RTSEC
         SRAD -> SCRAD (Scaled Radiance)
                 TSEC -> IRTSEC

        import_netcdf: fix scale_factor, 
        import_netcdf: data can be in anytype (old: float only)
        import_netcdf: calculations are in double precision, before returning float[]
        import_netcdf: fix scaling: old: required offset and scaling to be set
        import_netcdf: parallelized code
        -new_grid: lambertc ignored the -new_grid_winds option.
        -ndates: check to prevent of overflow of inv_out[]
        added char *base_inv_out(void) to check size of inv_out that is used
        INV_BUFFER increased to 50000 (from 30000)
        -grid: lambert conformal: remove "mode %d" which printed out res flag, added " " after m
        -grid: Albert eq area: remove "mode %d" which printed out res flag
        -AAIG output filenames include ensemble info (Manfred Schwarb)
        initialize nc_time_step (doesn't appear to need it) but compiler complains (Manfred Schwarb)
        -fix_CFSv2_fcst:  fixes the encode the number of ensemble members (Manfred Schwarb)
        reduced-_gaussian_grid: fixed declaration of enum interpol
        note: cubed_sphere2ll is not complete
        added: int sub_angle(char *))   old: grb2.h used int4(p) to read subangle
            new: grb2.h uses sub_angle(p) to read subangle
            handles case: subangle == 0 -> use 1, subangle == undefined -> use 1e6
            this changes subangle displayed in -v -grid
        removed tab from ip2lib_d/splat.f90
        new_grid: added checks for thinned grids, staggered grids, and ploughing order
        -ens_processing: fix d75 (typo) .. better accuracy for 75 percentile
        -ens_processing: will not do illegal memory reference to ens[save->n_ens]
            (value is multiplied by zero, so value is effectively not used)
        -ens_processing: k2 is made private
        -ens_processing: OpenMP structure revised to eliminate non-reproduceability with gcc 6.3.0
               testing of reproducablity OMP_NUM_THREADS > 1 same as OMP_NUM_THREADS=1
            gcc 4.4.8    good    testing of wgrib2 v2.0.8
                gcc 4.8.1    good
            gcc 5.3.0       good (bad with wgrib2 v2.0.9beta)
            gcc 6.3.0       bad
            intel 18.0.1    good
          comment: I don't see why the original OpenMP pragmas were wrong aside from the k2
            not being private.ZZ

        summary: wgrib2 v2.0.8 -ens_processing should be avoided
            1. newer versions of gcc have problems when OMP_NUM_THREADS > 1
            2. 75% interpolation between 2 points is inaccurate
            3. precip trace amounts is bad (cpc private)
            4. possible seg fault (array out of bounds multiplied by zero).
        Dump.c -d, added dump_offset from George Trojan
        fseek_file: works with pipe is whence is SEEK_CUR, needed by -d dump_offset
        init.c wgrib2.c: updates for -d dump_offset
        ffopen.c change mode of file by freopen: (George Trojan)
        added: geolocation variable that indicates which geolocation package created lat/lon values
        added: -geolocation which prints out which geolocation package created lat/lon values
        added: -new_grid_format
        makefile: fix typos to make USE_G2CLIB work, submitted G. Trojan
        -rpn:    old: (number)(characters) -> number 
            new: (number)(characters) -> error
        changed grib2/makefile and grib2/lib/makefile to make shared library libwgrib2.so,
            for COMP_SYS=gnu_linux in use by python interface
        wgrib2lowapi.f90: changed c_char -> c_signed_char for buffer
        splib: ncpus.f -> ncpus.F .. now compile when compiled without OpenMP
        splib: #if USE_SPECTRAL == 1  in polatess4.f90 -> polatess4.F90
        splib: #if USE_SPECTRAL == 1  in polatesv4.f90 -> polatesv4.F90
        RPN.c logf() -> log()  fix bug in gclib 
                RPN.c  more parallel code
        wgrib2.c: flush after writes to inv_file
        -rpn_rcl: fixed if (rpn_data == NULL and rpn_n != 0) should never occur
        makefile, New_grid.c minor fixes George Trojan
        mk_gdt.c: added grid 101, added \x03 to interpreted bytes
        new_grid() ip2lib_d version, if wind_rotation == undefined , use wind_rot of old file
        ffopen.c(): changed wgrib2_free_file(): old would remove file from linked list if not in use
            new: will close file and removee from linked list if in use/unused
        f_new_grid(): fix up wind_rotation.  should be set in phase == -1
        on setjmp: fclose(&infile) George Trojan
        added: -S_out .. needed for python
        set_metadata_str(): now allows metadata of form (d|D)=....... besides n:byte:(d|D)=...
        f_last(), f_last0(), added repeat_inv_out(): allow -last file1 -last file2
        fixed *new_mem_buffer(int n, size_t size) when buffer is unallocated. until pywgrib2_s,
          nobody can closed a memory file.
        wgrib2_free_file: removed warning if file not found
        grib2/makefile  --fast-math -> -ffast-math George Trojan
        grib2/makefile, ftp_api/makefile: changes to get make -j N to work with gcc and aocc
           Tried making it work before but compiling was too slow with hard disks and a slow cpu.
           With newish home system with nvme and 3600 cpu allowed me to make the many trials.
        fixed: ffopen() works with all models r/w/a
        Mem_buffer.c George Trojan fixed errors
        KMA local levels and local grib table added, Dong-il Jang
        added extern to init.c and wgrib2.c for following variables: run_flag, free_gribfield, 
            grib_data, found by George Trojan using gcc v10.x
        GDT.c:  added free list_opt, George Trojan
        mk_gdt.c: added Space View Perspective or orthographic, George Trojan
        mk_gdt.c: minor cleanup
        PyInv.c: added for pywgrib2_xr, George Trojan
        makefile: check for f77, quit if used
        added fatal_error_lu(..)
        added check for memory allocation in rd_seq_grib.c  found by George Trojan
        wgrib2.h changed INV_BUFFER from 50000 to 100000
        added pyinv: for pywgrib2_xr and pywgrib2_lite, George Trojan, expermental
# v0.2.0.8b9   12/2018 - never released v2.0.8
        -ens_processing: fixed spread calculation (one pass -> 2 pass calculation)
        -ndate, -ndates: added with mn (minutes) support
        -set_date, -time_processing: now supports mn time offsets
        added spectral option to -new_grid
        new_grid: removed loop to determine ibi
        -set_ftime2: works with pdt 2, 3, 4, 5, 6, 9, 10, 12, 13, 15, 41, 43
        -alarm will clear alarm in finalize stage
        init.c added include <string.h> in
# v0.2.0.7   12/2017 -----------------------------------
        report that missing. zlib.h in RedHat system, changed makefile which may 
                   but probably doesn't fix the problem
        update Code_Values.c
        Time_processing.c: do_ave .. quit if save->n_fields == 0
        added -ens_processing
        Level.c update code table 4.5 (level_table[]), replace CodeTable_4.5_ncep.dat 
            by ncep_level_table[] (needed by -set_lev) and makes codes more consistent
                    eliminated need for CodeTable_4.5_ncep.dat and get_code_Table_4.5_ncep.sh
            simplified special ncep levels
        -set_lev: fixed lowest level 11% integrated cloud cover
        -set_lev: added (level1) - (level2)
        ftn_wgrib2api: added grb2_UNDEFINED, grb2_DEFINED_VAL(), grb2_UNDEFINED_VAL()
        -set_lev: added %g in sequence (ncep level 241)
        net result: inventory levels is updated, -set_lev handles more levels
        makefile: gfort, icc, ifort more info in -config
        wgrib2(argc, const char **argv) => wgrib2(argc, char **argv)
        added -box_ave
        ncep grid 184 added: C. McGill
        grb2_inq(): bufer size was 53 instead of 71.
        move -fi from Match.c -> Fi.c
        move -end from Match.c -> End.c
        wgrib2api: grb2_inv: grid/desc, -last -> -last0
           no affect on good code, less space used code with non-unique matches
        prob: added fcst number/total
                   prob >2.54 -> prob >2.54:prob fcst 255/255
        wgrib2_get_mem_buffer_size(int n): return 0 instead of -1
             size_t is unsigned
        bug in sub_dt(..): if subtract time, and result should be dec 31
            bug:  dtime > jday -> dtime >= jday
            bug affects: -set_date when using -set_date -Xhr
            and the reference date should be dec 31.
            bug affects: verfification time when using negative forecast time (rare) and
            verification time should be Dec 31.
        wgrib2api.f90: changes len=100 to len=300 for longer file names
        USE_IPOLATES = 0,1,3 (new): changed makefile, Config.c ncep_grids.c mk_kgds.c New_grid.c
        -new_grid_winds: old-needed IPOLATES to be installed. new-IPOLATES does not need
            to be installed, needed for -submsg_uv
        wgrib2/makefile: removed references to NCEP_local_levels_test.h as no longer used
        added copy_dbl_data(..)  same as copy_data(..) except for double rather than float
        support for PDT 70 (post processing)
        sec3_grids.c sets wind rotation to earth
        modified -match_inv_add to use 2 arguments to the option
        added diagnostics to -merge_fcst: same_sec4_for_merge(.) when fails to merge
        fixed -set_radius 3:major_km:minor_km
        -set data_* and -set data_+ added
        closest(..): 2G+, some openmp speedup
        removed OpenMP from  gctpc_ll2xy.  gctpc_ll2i
        rot_regular2ll (returns lat-lon of rotated lat-lon grid) OpenMP enabled.
        fixed Warn_old_g2lib.c, HEADER swapped 0:inv (from Dusan Jovic)
        rm jasper-1.900.1 jasper-1.900.1.orig from jasper-1.900.1-14ubuntu3.2.debian.tgz (from Dusan Jovic)
        fix rpn smth9r
        makefile: rm tmpz.tar, ${cwd}/share, ${cwd}/tmp (from Dusan Jovic)
        Manfred Schwarb made his usual no-changes-needed contribution.  He
                  fixed and updated get_gribtab.sh which updates the gribtable.  Some
                  corrections and units are made more consistent. Thanks Manfred.
        -new_grid: correct file name in error message if could not open output file
        grb2_inq(..) fix error message if no message matches.
        -ens_processing: do all calcs from sorted data (speedup)
        Ensemble.c: added more statistics, set code_table 4.3
        updated code table table 4.3
        changed makefile: cd ${proj4} -> cd ${proj4dir}   thansk Simon Horton
        changed makefile to use latest netcdf4 and hdf5 v4.6.1 and 1.8.20
           note: hdf5 v1.10 can read v1.8 but not vice versa. stupid.
        ftime2: change "ave(0 min fcst)" to "ave anl", like ftime1, set_ftime2 already works
        added to -set_lev "n-m hybrid layer", needed for fv3
        iplib2_d: change splat -> ip_splat
        wgrib2api: added get_(ref/start/end)_edate, dynamical allocation of @mem:XX
        change number of memory buffers to 30, should fix wgrib2api to read N_num_memory_buffers
           from wgrib2.h file
        Level.c and Mod_grib.c added %g (Eta|logarithmic hybrid|hybrid height|hybrid pressure) 
                   level and layers, needed by fv3-core
        Grads.c: added hyp and hyh (hybrid pressure level/layer and hybrid height level/layer)
        changed Ftime2.c so that when NCEP prints TMIN/TMAX
                   old: 264:30420217:d=2004021912:TMIN:2 m above ground:66-72 hour missing fcst:
                   new: 264:30420217:d=2004021912:TMIN:2 m above ground:66-72 hour fcst:
                   the missing refers to a missing statistical operator (ex. ave, acc, min, max)
                   this verion is 1 hour newer than v0.1.7.8 .. for operations
        RPN.c: old: -rpn :asasdf was ignored, now a warning
        netcdf4 - change hdf5 1.8.18/1.8.18 to 1.10.4
           problem with newer gcc (ubuntu) with v1.8.20, however, 1.10.x
           may produce files that are not compatible with 1.8.
        Aerosol.c if (pdt < 44 && pdt > 48) -> if (pdt < 44 || pdt > 48) from Manfred Schwarb
        Time_processing.c duplicate  else if (pdt == 12) from Manfred Schwarb
        Reduced_gaussian.c removed extran extern output_order_type output_order; from Manfred Schwarb
        dec_png_clone.c void user_read_data_clone(.... png_uint_32) ->  void user_read_data_clone(... png_size_t)
            from Manfred Schwarb, worked on 32 bit machines because png_uint_32 == png_size_t
                        worked on X86_64 because X86_64 calling conventions transferred arg in a 64-bit
            register for both microsoft, linux and AIX(?) calling conventions
        makefile, config.h and Config.c: list version of hdf5
        11/2018 updated gribtab  manually changed CLWMR to CLMR
        Sec3.c added static print_stagger(), more grids with stagger
        -ens_processing: fixed bug in setting number of ens members
        -N_ens: fixed to print nothing if doesn't exist
        -import_text, open changed from r+ to r
        -import_ieee, -import_bin, open changed from rb+ to rb
        added const char *wgrib2api_info() .. library info
        makefile: compile netcdf4 without OpenMP
        fixed same_sec4_but_ensemble .. f1..f4 were off by 1, no affect on released codes fixed ens_processing
# v0.2.0.6c   2/2017 -----------------------------------
        added -set table_4.230 number   and -set table_4.230 string  chemical types
        f_misc:  chemical=chemical_40009 => chemical=40009
        CodeTable. 4.6: pdt 40 and 42 identified as ensemble
        -set_num_ens: works with any PDT with code table 4.6
        error if g2clib == 1 and decoding png field
           decoding png is ok
           decimal scaling is set to decimal scale of last constant field or 0.
                   This causes an error in -grib (BAD) or using the input scaling parameters (Not Great)
                   The BAD effects are rare because pngs are rare and people rarely decode and do -grib at the same time
                   The Not Great problems are only rare vs rare**2.  This involves a change a precision
                   which could results in a loss of precision.
        -set_grib_type now understands complex1-bitmap (c1b), complex2-bitmap (c2b), complex3-bitmap (c3b)
        -mysql_speed: size of character strings for table data changed from 1500 to MAX_SQL_INSERT=3000
                    user added an additional variable to a 40-level GFS output and wgrib2 crashed.
                    code should be updated to keep "row" from overflowing.
        makefile: added -I../include to compiling libpng  (needed by Windows 10, WLS)
        makefile: added code so that gfortran on MacOS could find gfortran.dylib
        grb2_wrt: old: order='ns' -> order='we:ns' or order='ns', doc should say we:ns
        -ftime2: some fixes
        new makefile:
            old libwgrib2.a uses thinned archived which is a gnu ar feature
            libwgrib2a_small.a -> libwgrib2x.a because unix ar has 15 letter limit
            added lib/makefile
        Set_ftime2.c: allow -set_ftime2 "0-1 month ave anl"
        same_sec4_not_time: all pdts
          better diagnostics: same_sec4_not_time(sec, sec) -> same_sec_not_time(int mode, sec, sec)
        better diagnostics: same_sec1_not_time(sec, sec) -> same_sec_not_time(int mode, sec, sec)
        added timing='6 hour fcst' into wgrib2api: grb2_wrt(..)
        changed fatal_error if n=0 (statistical processing) to warning
        seperate n=0 and memory size to different error messages (stat processing)
        -ncep_norm: changed debug output is output with v98 instead of v99
        several: tried to make ** Warning: reference time includes non-zero minutes/seconds **
            better, turn off if f_T(..) is called
        f_new_grid(..):  improved error message if ipolates fails
                   if 1st or 2nd grid is smaller than grid cell of 2nd or 1st grid
        -ftn_api_fn0: output format changed from i8,5(1x,i8) to i8,5(1x,i11)
        grb_inq(..): read of ftn_api_fn0 changed from i8,5(1x,i8) to i11,5(1x,i11)
            this allows reading of grids bigger than 99,999,999 points
        -v -varX : the output has been reformatted for local variables have the format
                          var discipline=0 center=34 local_table=1 parmcat=1 parm=203 
        -set_var(..) now understands: -var (undefined variables) and -varX formats
                   var discipline=0 center=34 local_table=1 parmcat=1 parm=203 
                   var discipline=10 master_table=2 parmcat=0 parm=11 
                   var10_2_1_7_0_11 
        small_domain(..) use GDS_change_no to avoid duplicate calculations
        small_domain(..) for lat/lon, mercator, only scan axis to save time
        fwrite_file(..): added ferror(..) after fwrite
        fread_file(..): added ferror(..) after fread
        mk_bms(..): optimize code by finding 1st undefined grid point value
            added check for malloc(..) working
        -set_pdt: supports aerosols pdt=46, 48
        -set_ftime2: fixed error message Set_ftime2: no match string=$s
        -set_pdt, if len specified, do not add extra bytes 
              if len not specified, add extra bytes for n time ranges
        f_misc: print aerosol size for pdt 44..47 (only did 44 before)
        f_spatial_proc(..): pdt 15, changed output format, more info
        -set_metadata, -set_metadata_str: uses f_set_ftime2 rather than f_set_ftime / f_set_ave
        f_set_pdt: moved code into new_pdt(..), so other codes can call
        -set_pdt: supports  minutes_of_observational_data_cutoff_after_reference_time
        -set_pdt: supports model version date pdt 60, 61
        -f_Match_inv, f_match_inv, replace CALL_ARG0 by call_ARG0 (modern)
        Set_metadata.c replace CALL_ARG0 by call_ARG1 (modern)
        f_(set|get)_(byte|hex|int|int2|ieee): Boi used octet == 0, so I added error checking
            for octet <= 0, slightly changed error message for out of bounds section
        Config.c:  make #include <jasper/jasper.h> conditionally compiled
        added -time_processing
        changed: -ave -> -ave0, -fcst_ave -> -fcst_ave0
        added: -ave, -fcst_ave, both point to -time_processing
        axes_earth, radius_earth: change GRS80 and WGS84 minor axis .xxx m
        -radius now prints out major/minor axes
        -radius output for user define oblate spheroid changed, consistent with other new output
        -rpn print_ave,print_rms,print_diff,print_corr will work when lat not defined
        -rpn_rcl, -rpn_sto, bug fix: do not set decode=1 
        makefile: clean now cleans ftp_api
        default vector fields: added USSD/VSSD
        added -alarm, terminates wgrib2 after N seconds by the alarm function
            added for web processing (nomads) to terminate jobs that may have
            stopped running because of broken I/O links
        added code_table 3.2 for grid def 101
        modified Earth.c as grid def 101 does not support user major/minor axes
        sec3_lc: change LatSP 0.000000 LonSP 0.000000 -> LatSP -90.000000 LonSP 0.000000
             change definition of latitude of southern pole                         
             THIS CHANGE REQUIRES TIN for interpolation products
        support for grid 101: -grib_out_irr2, Sec3.c
        updated documenation: undefine_val.html to reflect that the code can undefine a range
        added -import_netcdf
        Changed -alarm: text description, removed un-needed test
        added -match_inv_add, so can customize match_inv/Match_inv
        makefile: fixed clang support
        Ftime2: defined left and right in in unsupported cases
        received space view perspective grid with a satellite not on 0N OE but 0N 41.5E
                     which exposed some problems.
           space_view.c: remove conversion of lop to radians
           space_view2ij.c: fix value of lap, fix corrections of lap, lop to radians
           note: codes still requires lap, position of satellite, to be on equator
        get_latlon(): return error code instead of 0
        geo.c improved error messages, a little OpenMP for mercator2ll
        set_pdt(): removed char *sec4 because it was unused
        RPN.c xave and xdev fixed after bug introduced signed -> unsigned conversion
        RPN.c some missing 2G+ conversions fixed
        Sec3.c: in -grid for thinned Gaussian grid, remove print of dlon .. dlon doesn't apply here
        cyclic(), handles thinned Gaussian grid, handles 4G grids   .. for incorrect
                 ECMWF files. Lat/lon is assumes a global Gaussian grid
        updated gribtable
        fix: rpn_rcl, rpn_sto: Aug  8 2017: removed decode=1 from rpn_rcl and rpn_sto
                  12/5/2017 added it back
        Small_grib.c: added check for thinned grids. Never should be encountered.
        complex_pk.c: cleanup code, change so that old g2lib can read c2 and c3 files.
        wrtieee.c: fixed sizeof(ieee-float) = sizeof(float) to 4.
           I can't think of a current machine for which this is not true.
        added: -reduced_gaussian_grid, transform from reduced Gaussian to full Gaussian grids




# v0.2.0.0  6/2014 -----------------------------------
        -gctpc changed from inv to misc option
        -merge_fcst: if N == 0, output every merge step, for S2S (tigge)
        -set_metadata: fixed problems made in the 2.0.0 release.  

# v0.2.0.1  7/2014 -----------------------------------
        Level.c added more fixed levels
        support for PDT=60,61
        alias pdt=code_table_4.0
        set_pdt(), copies all metadata for common pdts (-set_pdt +PDT) not complete
        -Sec4, removed code for extra info when PDT=4.0
        set_pdt(), fixed default sizes esp stat processing PDTs, added more PDTs
        enable staggered grids (gctpc)
        proj4_get_latlon: lambert, lat-lon, mercator, nps
        added ncep grids 4 and 45 to ncep_grids.c
        -s_out write new-line at end
        -nl_out is now inv_output function
        geo: regular2ll: 1x1 grids allowed
        updated code_table_4.3.dat
        updated code_table_4.10.dat
        updated: prod_def_temp_size.c
        add option -hybrid
        f_ctl_ens: fixed and updated pdt==2 and pdt==12 are ensembles, added new pdts
        updated CodeTable.4.7.dat
        support for pdt 4.6 and 4.10, added -percent, percent_value(), percent_value_location()
        support for pdt 4.6 and 4.10, in ctl_inv (Grads.c)
        fixed cname.c units for pdt 4.6 and 4.10 should not be %
        ffclose, by John Howard, ffclose_finished WND
        added ffclose to -bin -ieee -text -spread -grib -GRIB -import_ieee, -import_bin, 
            -import_text -csv -cress_lola -ijbox -new_grid -small_grib -ijsmall_grib
            -grib_ieee -ncep_uv -set_metadata -grib_out -lola -s_out -inv_f77 -inv
            -print_out -nl_out -NCEP_norm -merge_fcst -ave -fcst_ave -irr_grid
            -grib_out_irr -eof_string -eof_bin
        EOF.c: if wgrib2 called with no arguments
            old: eof_bin and eof_string were called
            new: err_bin and err_string are called
               if f_h is called
            old: eof_bin and eof_string were called
            new: err_bin and err_string are called
            eof_string, eof_bin are called with mode -2  instead of explicitly
            err_bin, err_string: not called if no error
        all ffopens are paired with ffclose
        Match.c free memory, make match_count a global variable.
        rd_inventory: split from wgrib.c and put in own file, reads from input rather than stdin
        setup options will get finalized, so to close files
        pdt 4.44 only has 2 octets for forecast time!
                   set_ftime .. forecast time from int -> uint
        added declaration Cyclic.c, dummy function f_proj4 from John Howard
        moved scaling() from Precision.c to Code_Values.c added png, complex spectral
            and simple log-pre-processed
        change exit(0) to return 0 in main()
        CALLABLE_WGRIB2: setjmp/longjmp, exit -> return, fopen -> ffopen (input)
        CALLABLE_WGRIB2: -persistant_file
        replace fopen by ffopen in options, AAIG, i_file,
        f_h does not print out help message
        setup_user_gribtable.c, close input file
        -unix_time is now compile time option for John Howard
        converted some strcpy -> strncpy
        is_match: reorder loops for speed, add OMP for one loop
        added Match_fs package, like Match but for fixed strings
        wgrib2.c change to add Match_fs package
        wgrib2.c reorder search for option names
        initial support for code table 1.5 and 1.6
        wgrib2.c replaced seq_input into enum in_dev_type in_type;
        small_grib: unsigned int new_sec[7] -> new_sec[8] .. pretty typing
        rd_grib_msg*  updated to read more than 1 file at a time
        increased size sec[], sec[9] = last valid bitmap .. only by sec[] used to read
        added error messages Gctpc.c
        unpk_run_length.c: David Bindermani
            while (vals[i]> mv && i < nvals) -> while (i < nvals && mvals[i]> mv)
        setup_user_gribtable.c added fclose(input)
        set_metadata: fixed format of scanf
        cleanup of defined but unused variables (found using cppcheck)
        Scan.c: added parameters to routines, make it callable by f_import_grib()
        added -import_grib
        -fcst_ave, -ave, -wind_dir, -wind_speed, -merge, -ncep_norm : old: picked 
                    up scaling from first record.  Problem when first record was constant field.
            set use_scaling=0
        -fcst_ave, -ave: replaced undo_scan_order, saved many mallocs/frees
        can now write complex-packed files with bitmaps (for NAM), added -set_bitmap
        added MAXUW and MAXUV to list of vector fields
        Ensemble.c Manfred Schwarb: treat number_of_forecasts_in_the_ensemble(sec) as signed
            instead of cast to unsigned, (no change in code behavior)
        Level.c Manfred Schwarb: if (type <= 192) -> if (type < 192)  problem if level = 192 (local level)
        Merge.c Manfred Schwarb: treat code_table_4_10(sec) as signed instead of case to unsigned
            (no change in code behavior)
        NCEP_norm.c Manfred Schwarb: removed unused variable pdt, typo in format
        Prob.c Manfred Schwarb: fixed format print hex number with %u format 
        f_new_grid_vectors() accepts lists
        NCEP_uv.c: put struct local_struct into function definition (looks better)
        NCEP_uv: better error messages
        match_inv: added -varX, -T, -start_FT and -end_FT, decided against -VT
            -VT has same value as -start_FT, -VT and -vt have similar vt=(number) entries
        complex_grib_out: strange nam file: RIME had val=(1e+10+i*2^1)*10^-10, i=0..511 (#bits=9)
            convert to data to integer vector  u[i] = floor((data[i] - ref)*scale + 0.5);
            when using decimal scaling, ref is not quite the min value under all compilation
            options (could be 64-bit register vs 32-bit memory value).  The scaling factor is 
            so large that u[i] could be negative!  Added a step to make sure u[i] is >= 0
        -fcst_ave, -ave, -wind_dir, -wind_speed, -merge, -ncep_norm : restored to old
            behavior, scaling from first record. 
        added -code_table_4.8
        -set added table_4.7 table_4.8
        -rpn: added smth9g and smth9r
        -set_percentile: added, added "N% level" to set_metadata
        Ensemble.c pdt=2 changed output to make clearer
        -pdt old output: code table 4.0=1 Individual ...  new: pdt=1
        added -pdt to match_inv
        -set_prob: added, added support in set_metadata
        -set_ens_num: now callable from code
        -set_metadata:  supports ens=(+/-)number, ens=hi-res ctl ens=low-res ctl, X ensemble members
           note:earlier versions ignored "ens=low-res ctl"
        polar2ll: make openmp gau2ll
        Config.c: would say that gctpc was default even if not

# v0.2.0.2  3/2015 -----------------------------------
        updated New_grid.c, vectors and default_vectors

# v0.2.0.2a beta  3/2015 -----------------------------------
        -rpn: smth9r would do a wrap-around like smthg, fixed
        moved set_metadata() from Mod_grib.c to Set_metadata.c, added -set_metadata_str
        added UICE/VICE to list of vector fields in New_grid.c, George Trojan
        rd_inventory: changed save inventory in buffer for (future) grep operations
        added -fgrep, -fgrep_v, -egrep and -egrep_v
        fixed wgrib2 so that "wgrib2 file -last file.txt -s" works, if -last is 1st option,
            picks "message no:position"
        fixed makefile: added mkdir include
        space_view.c space_view2ll() Leon Majewski found that lop was incorrectly defined.
           lop = int4(sec[3]+38); -> lop = int4(sec[3]+42); Not noticed because EUMET
           satellite has lap = lop = 0.0.
        CW2: f_inv open files a+ and w+, need to opens
        rewind_file: changed from rewind() to fseek() to get error code
        CW2: fixed ffopen() so if error, link list is in good shape for future call
        forecast_time_in_units now signed: Code_Values.c Merge.c, Set_date.c 
            Set_ts_dates.c, Mod_grib, Verftime.c
        added sub_dt, add_time: dtime can now be plus or minus
        Verftime.c use code_table_4_4() to check for no forecast hour
        f_rpn(): do not cleanup on mode == -2, allow callable wgrib2 to keep
            registers between calls
        updated to libpng from v1.2.50 to v1.2.52
        changed GB2_Subcenter(sec) from (int) int2(..)  to (int) uint2(..)
        changed GB2_Center(sec) from (int) int2(..)  to (int) uint2(..)
        wgrib2(): if malloc of data fails && callable wgrib2 .. set ndata = 0
        EOF.c: many small changes
        wgrib2: fixed setjmp code .. must save current stack frame!!!! else seg fault
            on subsequent calls to wgrib2 && fatal_error!!! 
        rd_inventory: make buffer[STRING_SIZE] -> buffer[INV_STRING_SIZE]
        updated New_grid.c, vectors and default_vectors so can compile without IPOLATES
        removed unused variable i from Set_percentile.c
        updated Match.c  extern int use_ext_name is now defined if REGEX is off
        added -ftn_api_fn0
        cleanup wgrib2.c
        free memory Undefine.c Set_val.c Set_ensm_derived_fcst.c Import_grib.c Cress_lola.c
            Fix_CFSv2_fcst.c
        f_i: removed comparison of rd_inventory == stdin as done in ffclose
        int8: changed arguments from unsigned char * to unsigned const char where applicable
        ijsmall_grib, small_grib: now respects append flag
        many: replace FILE *xyz by struct seq_file *xyz and struct seq_File xyz
            to support @mem: files
        tosubmsg: better test for writes to pipes which is not allowed
        -last does not include \n
        -inv (write inventory to file) is changed from misc to setup
        wgrib2.c changed fprintf(inv_out,..) to fwrite(...,inv_out)
        inv_file changed to struct seq_file
        user_grib_file, all rejected table entries are printed out
        cname: use local table if (discipline, parmcat, parmnum) is member of [192..254]
        many files: changed FILE *file to struct seq_file *file.
        changed gctpc to remove warning about som_series by removing static som_series from proj.h
           and adding them to sominv.c and somfor.c
        Mem_buffer: data structures are statically initialized here instead of call to 
            init_mem_buffers, this allows external routines to initialize memory file.for CW2
        init_mem_buffers only checks that initialization was done
        RPN.c initialize rpn_n[] and rpn_data[] at compile time rather than at run time
            allows external routines to initialize RPN registers
        -full_name: fixed, now full_name = -ext_name "." level
        added U-GWD, V-GWD to vector list
        wrtieee: OpenMP the conversion loop works now.
        -import_grib: fixed definiton from read grib1 -> read grib2
        swap_buffer: OpenMP version
        added MRMS grib table  https://www.nssl.noaa.gov/projects/mrms/operational/tables.php
        change makefile: openmp and no gfortran is ok, MAKE_FTN_API
        prt_stat_tr: "-1-2 hour ave fcst,anl++" -> "-1-2 hour ave anl" affects inventories
        complex_pk.c: error when writing msg with all undefined (code table 6.0 is undefined)
        *** this bug affects writing of fields that are ALL undefined in complex packing ***
        complex_pk.c: OpenMP directives added, renamed igrp -> itmp
        added min_max_array() - OpenMN min/max of an array
        f_min, f_max, complex_pk.c call min_max_array()
        updated jasper to latest ubuntu code (security fixes)
        updated gribtable - thank Manfred for the updated get_gribtab.sh
           many formating changes to units/description
           some differences from NCO tables because of dup names
           SOILM -> SOILM and SOILM_
           WILT -> WILT and WILTPT (WILT is old)
           CDCT -> CDCT and CDTYP
           TCOND -> TCOND and TCONDold
           TCOLW -> TCOLW and TCOLWold
           FRICV -> FRICV and FRICVW
        makefile: ubuntu 12.10 /usr/lib/libgfortran is gone
           get location of libgfortran from compiler
           link ipolates library before fortran library
        Mem_buffer.c gmerge.c: cleanup as indicated by clang
        added code_table_4_1_location and code_table_4_2_location
        preliminary support for local JMA table 4.50008
        New_grid.c minor OpenMP additions
        update gribtable:
          get_gribtab.sh update by Manfred Schwarb
                  make_gribtable.sh  by Manfred Schwarb
          manual change to gribtab
          0:1:0:255:0:0:6:17:TCOND:Total Condensate:kg/kg -> TCONDold
          0:1:0:255:0:0:6:18:TCOLW:Total Column-Integrated Cloud Water:kg/m^2 -> TCOLWold
          0:1:0:255:0:0:6:19:TCOLI:Total Column-Integrated Cloud Ice:kg/m^2 -> TCOLIold
          10:1:0:0:0:17:FRICV:Friction Velocity:m/s  -> FRICVW
          2:1:0:0:0:26:WILT:Wilting Point:kg/m^3  -> WILTPT
          manual change to gribtab
        new_grid_lambertc: check return codes (long_i)
        misc changes to remove warning .. unused variables
        ieee_pk.c simple_pk.c Small_grib.c remove #include <jasper/jasper.h>
        added dec_png_clone.c changes to makefile and unpk.c: Need to make
            wgrib2 totally independent of g2clib in default mode.
            Problem was NCEP Central Operations (NCO) had g2clib compiled with
            -D __64BITS__ which told g2clib that ints were 64 bits even
            though ints were 32 bits!  Normal people don't do this.  
            So if you linked in NCO's libraries you had problems in the API.  
            However, NCO requires you to link in their libraries.  I have no idea
            how many external versions of g2clib this will affect as it is unknown
            if the NCO makefile for g2clib is widely distributed.
        Small_grib.c small optimizations
        simple_pk.c:  OpenMP optimizations
        grads.c: f_lev0() added adcl
        f_rpn():  number starts with +/-/0-9/.   (period is new)
        free_mem_buffer():  added mem_buffer[n] = NULL; fixed seg fault with callable wgrib2 and mem files
        -last: old: rewind files, new: rewind all file types
        -rewind_file: warning if fails, support for memory files (@mem:0)
        added -error_count
        return code from wgrib2 is |= (all function calls in phase -2, finalize)
        -config: added more configuration details
        prelim Lambert Az. Equal Area projection: gctpc
        proj4.c  gdt==32769 is now restricted to NCEP
        changed order of calculation of lat-lon: proj4, gctpc, built-in
        -- new programming rule: assume OpenMP can use unsigned ints in for loops
        unpk.c  i and ndata are unsigned int
        Mysql_dump: make i,j unsigned int, format %d -> %u for ndata and npts
        updated level.c for more definitions
        update lev0 for more definitions
        -rewind_file -> -rewind_init, -rewind_final, -rewind_proc
        -close_persistent_file -> -transient
        -persistent_file -> -persistent
        wgrib2.c expanded error message for no input file
        added fatal_error_ss
        changed to new error message "too many grib files .. 1st=%s 2nd=%s"
        added error message for undefined option
        mem_init: set mem_buffer_size[n] = 0 on failed read
        f_AAIG(): change fclose to ffclose
        added preliminary version -AAIGlong
        Ftn_api.c: removed references to nndata
        enc_jpeg2000_clone: added #include "wgrib2.h" to add prototyping

# v0.2.0.3  11/2015 -----------------------------------
        fix comments in import_grib.c, improved error message in import_bin
        changed NDFD_gribtable.dat, set local table=1, NDFD variable now work
        allow -d '    1'   f_dump() trim leading spaces
        added -rpn_rcl and -rpn_sto .. for CW2 .. no need for geolocation
        parallelized finding min/max jpeg_pk.c
        -set_prob will not crash with unsupported PDT
        return void: changed add_bitstream and add_many_bitstream, finish_bitstream
        -stats is optimized, cos -> cosf for speedup, may give slightly different results
        updated cyclic():  staggered grids never return cyclic, updated help msg
        -stats is openmp optimized, cos -> cosf for speedup, may give slightly different results
        2G++ grids enabled: -f_lola, -set_ival, -set_ijval, simple_grib_out
        f_ctl_ens: went back to original version which was coded to GrADS documentation.
        complex_grib_out: removed u[], saved sizeof(int)*ndata bytes peak usage
        prelim: added support for JMA radar grid and template
        added f_JMA to f_misc
        complex packing: fixed, could not handle grid greater than 512M grid points
            limit should be 4G grid points, problem testing 4G limit because of limited memory
        complex packing: fixed, problem the group size could grow to >=  2**25
            (artificial data) and the group size could not be stored because
            of a limitation of the bitstream package.
        f_ens(): for pdt=2,12 (derived ensemble fcst based on ALL ensemble members)
            removed mention of cluster mean in CodeTable 4.7.
        -N_ens: if N = 255 (undefined), print -1
        -ensm_derived_fcst(): fixed case where pdt == 2 and 12
        -ensm_derived_fcst(): can now be called by set_metadata_str
        -set_metadata and -set_metadata_str will scan 3 more free format fields 5 -> 8
        new_grid: had to add sec1 in order to run mk_kgds. With locally defined grids,
           sec3 routines now have to look at the center.
        unpk_complex() some 2G+ modifications, test for more failed mallocs
        added BOM local grib table definitions, prelim (I made it up) grib variable names
        Scan.c: 2G+, speed-up, undef nx, ny = 0 or -1
        translation[] from int -> unsigned int: ave(), -ave, -fcst_ave, Scan.c
        to_we_ns: fixed problem which caused "undo_output_order: program error"
            would appear with wgrib2 ndfdfile -order we:ns -grib_out junk
        regular2ll: Gctpc.c: f_bin: -checksum: 2G+
        copy_sec(): size[] -> unsigned int*, copy_data(): fixed malloc to be 2G+
        -wind_speed, -wind_dir: added openmp
        changed extern int npnts -> extern unsigned int npnts
            many, changed malloc(ndata *sizeof(XXX)) -> malloc(((size_t) ndata) * sizeof(XXX))
        changed uint4_missing: old: return int , if missing return -1
            new: return unsigned int, if missing return 0.
        added get_nxny_, like get_nxny but nx, ny are unsigned int
        rd_grib2_msg_seq_file(): change j to from int to size_t, len_grib to size_t;
        unsigned long int -> size_t: grib_ieee(), Checksum, cksum(), f_GRIB(), NCEP_uv(), submsg_uv()
        add xdev to -rpn  xdev: deviation from zonal mean
        nx/ny -> nx_/ny_: AAIG.c
        remove f_spectral (satellite), -spectral_bands exists, works and provides more info
        support for pdt=4.32, 4.33, 4.34
        space_view_closest: return long int, error if iy >= nny (before: iy > nny), 2G
        closest(), latlon_closest(), space_view_closest(): from int to long int
        bug fix: Many thanks to Steve Sullivan (mathcom.com) who found a bug in complex packing 
            (complex_grib_out) and provided a bug fix.  Complex packing divides the string of grid 
            points into groups.  The minimum grid point value within the group is the group reference
            value.  In order to pack the group references, you need to find the maximum
            group reference value (grefmx).  The loop that finds the grefmx bypasses the reference 
            value of the 1st group.  This is a problem when the reference value of the 1st group is 
            the maximum reference values and the number of bits required to encode this value is 
            larger than number of bits required to encode the second largest reference value.  
            This bug is for versions 2.0.0 - 2.0.3.  This bug is more likely to occur when the grid 
            is small.  

# v0.2.0.4  2/2016 -----------------------------------
        updated to libpng.1.2.56
        f_match_inv: old match inv depended on verbosity, new: match_inv is fixed
        -proj4_ll2i: works for lat-lon grids, always writes to inv
        updated ExtName.c adds new NCEP defn, and adds process=X for val >= 192
        -lli2: 0 == out of domain, always write to inv
        -ave: support pdt = 2 and 12
        wrtieee(): initialize i
        added -set_date +10hr, set_date -12mo,
        -ave: pdt==1, fixed rt, sec4[50] = 1; 
        updated code for table 4.233, updated table 4.233, remove <sub> and </sub>
        support for pdt 4.45, 4.46, 4.47
        wgrib2.c: "g2lib/g2clib XXX decode may differ from WMO standard"
           replaced by "g2lib/g2clib XXX encode/decode may differ from WMO standard"
        FlagTable.c break; break; -> break;
        parallelize missing_points(), unpk_0, unpacking of simple packed fields
        mk_sec5_sec7(): fixed seg fault when number of grid points is 0, added test for mem allocation
            affects v2.0.3-v2.0.4
        jpeg_pk: if n_defined == 0, initialized min_val = max_val = 0.0, no change in validity of output
        ext_name: do not print out code table 4.3 if value = 255
        set_metadata: added   code table X.Y=Z  and flag table X.Y=Z
        -set table_0.0, -set discipline  now works
        unpk_run_length.c:  Takayuki Usui found that the code was off by one in reading the table
           values.  Suggested a bug fix.
        -config show the version of IPOLATES library
        makefile shows version of gcc
        iplib.2012:  interpolation to Gaussian grid and writing to stdout
           fails: wgrib2 IN.grb -new_grid_winds earth -new_grid ncep grid t62 - | wgrib2 -
                  debug write statement in gdswiz04.f gets written into grib message. Fortran and C
          buffers are not unified and problems occur on flushing the buffers.
          remove writes from gdswiz04.f and splat.f
        iplib.v3.0.0:  added as possible iplib, same output, faster (16s -> 15s)
          will make it the default iplib after more testing
        code table 4.15: added location function
        preliminary support for PDT 4.57
        set_pdt: copy percentile info, all probability info
        units.c: added a2code_4_10()
        new_grid: added ncep grid 194
        added struct full_date: make code nicer by storing date in a structure
        added Ref_time which is the same as reftime but uses struct full_date
        added Get_time which is the same as get_time but uses struct full_date
        New_grid.c added #ifdef CRAYCE for cray fortran compiler, cray compilers compatibility has not been tested
        tried using setvbuf() but removed change: minimal improvement vs portability?
        added --version as an alias to -version
        added f_ftime2 code as a replacement of f_ftime: bug fixes, easier to understand
        move f_ftime code to Ftime.c, in prep of eventual removal
        changed makefile so that zlib is static than dynamic linked
        changed makefile so that pngs grids can be up to (200M x 1M)
        updated gribtable (HWRF upgrade)
        Sam Trahan (HWRF upgrade) updated Spectral.c for the current defn of PDT 4.32 and mod ExtName
        Gregor Schnee, Daniel Lee both of DWD: added support for CCSDS/aec compression, template 5.42
        renamed min_max_array.c to openmp_util.c
        openmp_util.c: added min_max_array_all_defined()
        jpeg_pk: openmp to find min/max of data[]
        complex_grib_out: preprocessing are now all openmp
        complex_grib_out: problem when all grid points undefined, fixed  sec5[51] = 1; -> sec[41] = 1
            note: output is a valid grib file, possible problem when making file
        added Verf_time(sec, &date;
        AAIG.c AAIGlong.c: allow dx != dy cleanup (Manfred Schwarb)
        cleanup: format, small errors, compiler warnings, grammar Manfred Schwarb
        Manfred Schwarb: output from gctpc renamed gctpc_1.txt -> gctpc_error.txt  gctpc_2.txt -> gctpc_param.txt
        simple_pk: handle case of n_defined == 0, affects v2.0.4
        add_bitstream: can handle n_bits up to 32.  (previously was up to 25)
        fread_mem:  (void *) mem_buffer[n] + mem_buffer_pos[n] ->  (void *) (mem_buffer[n] + mem_buffer_pos[n])
        swap_buffer(a,b)  b changed from int to unsigned int

# v0.2.0.5rc  7/2016 -----------------------------------
        mv Setftime2.c and Ftime2.c to *.cc (code in development)

# v0.2.0.5    7/2016 -----------------------------------
        added -set table_4.5a, -set table_4.5b  (some MDL files used a bad code table 4.5b)
        removed error message from wgrib2_set_mem_buffer() and wgrib2_get_mem_buffer()
        changed return code for wgrib2_set_mem_buffer from 2 to 3.  unify return codes for set/get mem_buffer()   
        change N_REGS -> N_RPN_REGS, moved to wgrib2.h, set N_RPN_REGS=20
        fixed RPN.c to reflect new values of N_RPN_REGS (vectors had to be manually increased)
        changed grb2_inq() in wgrib2api.c: old: used rpn register 0,1,2   new: uses registers 17,18,19
        ftn_api grb2_inq(): added lastuse variable
        wgrib2.h: fixed int stagger(unsigned char **sec, unsigned int assumed_npnts, double *x, double *y)
        parse_time_range2: error if no match
        f_ftime(): removed \n in fatal_error message
        t ftime2_tr(): added fatal_error "Statistical processing bad n=0"
        RPN.c: dimensioned rpn_n[N_RPN_REGS], and rpn_data[N_RPN_REG] rather than default
                  which causes an error message when N_RPN_REGS is modified.  Now assume that
                  undeclared inits are 0 and NULL.
        main(): combined scans for input_files/inv_option and compile into one loop
          search for option names is done once instead of twice
        removed the (alpha) from -mysql header
        fixed header for -v97
        BOM gribtable: removed colons as they adversely affected g2ctl
        libaec 3.4 now used, Config.c changed
        new versions of netcdf4 4.4.1 and hdf5 1.8.17
        unpk.c for (i=0;i<pcmpt->height_;i++) { .. i replaced by ii .. undefined variable.
        unpk.c remove declaration for c when compiling Jasper
        init.c, rd_inventory.c: egrep* is only used when REGEX, bug report by J. Crockett
        stagger.c assumed_npnts -> unsigned int
        enc_jpeg2000_clone.c:  jasper v1.900.25+ changes ABI, need to comment out line image.inmem_=1
          in makefile, JASPER_1_900_25PLUS is defined in config.h
            at this point jasper v1.900.29 doesn't test cleanly on redhat
            but library works.
        added grib_max_bits: so set_metadata can look like inventory
        added precision: so set_metadata can look like inventory
        simplified set_bin_prec
        metadata string: added "encode %d bits"
        wgrib2(..): flush_inv_file:  so for CW2, inventory is readable
        updated level.c, lev0(..) for new fixed levels
        updated Ensemble.c for new table entries
        Ngoan Tan: updates to CodeTable.c, fill in some missing info for JMA pdt 50011 and 50008.
            note: I did not have any readable documentation to verify locations.
        Sec1: f_t(..): added warning if minutes or seconds is non-zero
        makefile: fixed problem: $TMPDIR is required to compile pnglib, fixed
        makefile: fixed problem with icc with openmp in compiling pnglib
        status_fopen(): now prints out usage
        mk_file_transient, mk_file_persistent: always return 0
        wgrib2_free_file: removes file from ffopen link lists if usage == 0
        main(); int err_4_3_count -> static int err_4_3_count = 0; so only 3 msgs for CW2
        -grid_def: will not free NULL, will work on all records
        makefile: TMPDIR is now set
        f_set_lev(): support of A-B m ocean layer (WMO version)
        level(): A-B m ocean layer, old: ncep version, new: ncep & WMO
        added code_4_230_location(..)
        makefile: fix ifndef ${..} =? ifndef ...
        makefile: zlib: make check fails with undefined symbol cygwin64

# v0.2.0.6    2/2017 -----------------------------------
        withdrawn before 48 hours to allow changes to ftn_api
        The following changes are so that metadata returned from grb2_inq(..) is of the
         format: D=YYYYMMDDHHmmss:var:lev:etc

         The rest is so that index file is a superset of that format and
         compatibility with the D= instead of d=

        set_metadata:  old str:  x:x:d=yyyymmddhh(mmss):etc
                       new str:  x:x:(d|D)=yyyymmddhh(mmss):etc
        grb2_inq(..,desc=str,.. old: str=`wgrib2 -s`    ie d=YYYYMMDDHH:var:lev:,etc
                                 new: str=`wgrib2 -S`    ie D=YYYYMMDDHHmmss:var:lev:etc
        grb2_mk_inv: old wgrib2 GRB -match_inv > INV
                 new wgrib2 GRB -Match_inv > INV   swap D= and d=
              so desc=meta, then meta can be used in a search
        grb2_inq: searches are now fixed strings rather than reg
                 use regex=1 to convert to regex searches

# v0.2.0.6a   2/2017 -----------------------------------
        withdrawn to fix problems with compiling on Macs. (6 hours of release)
        set_ftime2: uint missing -> unsigned int missing
                fix makefile so that it will compile on Macs .. thanks Daniel Bowman

# v0.2.0.6b   2/2017 -----------------------------------
        fixed code_4_230_location(..) broken in v2.0.6

# v0.2.0.6c   2/2017 -----------------------------------
        added -set table_4.230 number   and -set table_4.230 string  chemical types
        f_misc:  chemical=chemical_40009 => chemical=40009
        CodeTable. 4.6: pdt 40 and 42 identified as ensemble
        -set_num_ens: works with any PDT with code table 4.6
        error if g2clib == 1 and decoding png field
           decoding png is ok
           decimal scaling is set to decimal scale of last constant field or 0.
                   This causes an error in -grib (BAD) or using the input scaling parameters (Not Great)
                   The BAD effects are rare because pngs are rare and people rarely decode and do -grib at the same time
                   The Not Great problems are only rare vs rare**2.  This involves a change a precision
                   which could results in a loss of precision.
        -set_grib_type now understands complex1-bitmap (c1b), complex2-bitmap (c2b), complex3-bitmap (c3b)
        -mysql_speed: size of character strings for table data changed from 1500 to MAX_SQL_INSERT=3000
                    user added an additional variable to a 40-level GFS output and wgrib2 crashed.
                    code should be updated to keep "row" from overflowing.
        makefile: added -I../include to compiling libpng  (needed by Windows 10, WLS)
        makefile: added code so that gfortran on MacOS could find gfortran.dylib
        grb2_wrt: old: order='ns' -> order='we:ns' or order='ns', doc should say we:ns
        -ftime2: some fixes
        new makefile:
            old libwgrib2.a uses thinned archived which is a gnu ar feature
            libwgrib2a_small.a -> libwgrib2x.a because unix ar has 15 letter limit
            added lib/makefile
        Set_ftime2.c: allow -set_ftime2 "0-1 month ave anl"
        same_sec4_not_time: all pdts
          better diagnostics: same_sec4_not_time(sec, sec) -> same_sec_not_time(int mode, sec, sec)
        better diagnostics: same_sec1_not_time(sec, sec) -> same_sec_not_time(int mode, sec, sec)
        added timing='6 hour fcst' into wgrib2api: grb2_wrt(..)
        changed fatal_error if n=0 (statistical processing) to warning
        seperate n=0 and memory size to different error messages (stat processing)
        -ncep_norm: changed debug output is output with v98 instead of v99
        several: tried to make ** Warning: reference time includes non-zero minutes/seconds **
            better, turn off if f_T(..) is called
        f_new_grid(..):  improved error message if ipolates fails
                   if 1st or 2nd grid is smaller than grid cell of 2nd or 1st grid
        -ftn_api_fn0: output format changed from i8,5(1x,i8) to i8,5(1x,i11)
        grb_inq(..): read of ftn_api_fn0 changed from i8,5(1x,i8) to i11,5(1x,i11)
            this allows reading of grids bigger than 99,999,999 points
        -v -varX : the output has been reformatted for local variables have the format
                          var discipline=0 center=34 local_table=1 parmcat=1 parm=203 
        -set_var(..) now understands: -var (undefined variables) and -varX formats
                   var discipline=0 center=34 local_table=1 parmcat=1 parm=203 
                   var discipline=10 master_table=2 parmcat=0 parm=11 
                   var10_2_1_7_0_11 
        small_domain(..) use GDS_change_no to avoid duplicate calculations
        small_domain(..) for lat/lon, mercator, only scan axis to save time
        fwrite_file(..): added ferror(..) after fwrite
        fread_file(..): added ferror(..) after fread
        mk_bms(..): optimize code by finding 1st undefined grid point value
            added check for malloc(..) working
        -set_pdt: supports aerosols pdt=46, 48
        -set_ftime2: fixed error message Set_ftime2: no match string=$s
        -set_pdt, if len specified, do not add extra bytes 
              if len not specified, add extra bytes for n time ranges
        f_misc: print aerosol size for pdt 44..47 (only did 44 before)
        f_spatial_proc(..): pdt 15, changed output format, more info
        -set_metadata, -set_metadata_str: uses f_set_ftime2 rather than f_set_ftime / f_set_ave
        f_set_pdt: moved code into new_pdt(..), so other codes can call
        -set_pdt: supports  minutes_of_observational_data_cutoff_after_reference_time
        -set_pdt: supports model version date pdt 60, 61
        -f_Match_inv, f_match_inv, replace CALL_ARG0 by call_ARG0 (modern)
        Set_metadata.c replace CALL_ARG0 by call_ARG1 (modern)
        f_(set|get)_(byte|hex|int|int2|ieee): Boi used octet == 0, so I added error checking
            for octet <= 0, slightly changed error message for out of bounds section
        Config.c:  make #include <jasper/jasper.h> conditionally compiled
        added -time_processing
        changed: -ave -> -ave0, -fcst_ave -> -fcst_ave0
        added: -ave, -fcst_ave, both point to -time_processing
        axes_earth, radius_earth: change GRS80 and WGS84 minor axis .xxx m
        -radius now prints out major/minor axes
        -radius output for user define oblate spheroid changed, consistent with other new output
        -rpn print_ave,print_rms,print_diff,print_corr will work when lat not defined
        -rpn_rcl, -rpn_sto, bug fix: do not set decode=1 
        makefile: clean now cleans ftp_api
        default vector fields: added USSD/VSSD
        added -alarm, terminates wgrib2 after N seconds by the alarm function
            added for web processing (nomads) to terminate jobs that may have
            stopped running because of broken I/O links
        added code_table 3.2 for grid def 101
        modified Earth.c as grid def 101 does not support user major/minor axes
        sec3_lc: change LatSP 0.000000 LonSP 0.000000 -> LatSP -90.000000 LonSP 0.000000
             change definition of latitude of southern pole                         
             THIS CHANGE REQUIRES TIN for interpolation products
        support for grid 101: -grib_out_irr2, Sec3.c
        updated documenation: undefine_val.html to reflect that the code can undefine a range
        added -import_netcdf
        Changed -alarm: text description, removed un-needed test
        added -match_inv_add, so can customize match_inv/Match_inv
        makefile: fixed clang support
        Ftime2: defined left and right in in unsupported cases
        received space view perspective grid with a satellite not on 0N OE but 0N 41.5E
                     which exposed some problems.
           space_view.c: remove conversion of lop to radians
           space_view2ij.c: fix value of lap, fix corrections of lap, lop to radians
           note: codes still requires lap, position of satellite, to be on equator
        get_latlon(): return error code instead of 0
        geo.c improved error messages, a little OpenMP for mercator2ll
        set_pdt(): removed char *sec4 because it was unused
        RPN.c xave and xdev fixed after bug introduced signed -> unsigned conversion
        RPN.c some missing 2G+ conversions fixed
        Sec3.c: in -grid for thinned Gaussian grid, remove print of dlon .. dlon doesn't apply here
        cyclic(), handles thinned Gaussian grid, handles 4G grids   .. for incorrect
                 ECMWF files. Lat/lon is assumes a global Gaussian grid
        updated gribtable
        fix: rpn_rcl, rpn_sto: Aug  8 2017: removed decode=1 from rpn_rcl and rpn_sto
                  12/5/2017 added it back
        Small_grib.c: added check for thinned grids. Never should be encountered.
        complex_pk.c: cleanup code, change so that old g2lib can read c2 and c3 files.
        wrtieee.c: fixed sizeof(ieee-float) = sizeof(float) to 4.
           I can't think of a current machine for which this is not true.
        added: -reduced_gaussian_grid, transform from reduced Gaussian to full Gaussian grids




