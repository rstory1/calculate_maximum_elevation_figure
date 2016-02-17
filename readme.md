#Calculate MEF values for areas where we have terrain and/or obstacle data
#Used with Aviation map 
#
#Uses data from SRTM and NASR obstacle database
	To get started:
		#execute setup script
		#Installs dependencies, 
		./setup.sh
		
		#Download all SRTM data
		./getSrtmData.sh
		
		#Get the data archive from Dropbox
		wget --timestamping -erobots=off https://www.dropbox.com/s/z4iimyokif472wj/MaximumElevationFigureData.tar.xz?dl=0
		
		#Get the latest NASR database
		#I've included one or you can create fresh data with
		#https://github.com/jlmcgraw/processFaaData
		
		#Execute the script
		carton exec ./calculate_mef.pl /path/to/srtm/files
		
		#Convert CSV to sqlite database
		sqlite3 mef.sqlite < import.sql
		sqlite3 mef.sqlite < addIndexes.sql
		
		#Convert sqlite to spatialite
		cp mef.sqlite mef_spatialite.sqlite
		sqlite3 mef_spatialite.sqlite < sqliteToSpatialite.sql

I've also included the output as of 17 February 2016
        mef.csv
            Highest of terrain or obstacle MEF for each quadrant
        
        obstacles.csv
            Highest obstacle MEF for each quadrant
        
        terrain.csv
            Highest terrain MEF for each quadrant
        
        mef.sqlite
            sqlite version of data with indexes
        
        mef_spatialite.sqlite
            spatialite version of data
            
You'll notice that the SRTM data only goes up to ~60 degrees North latitude

May require GDAL and Perl bindings version 2.0+