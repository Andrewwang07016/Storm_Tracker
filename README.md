# Storm_Tracker
Track storms using data retrieval techniques.


// StormChaser.java import java.io.*; import java.util.Scanner;

public class StormChaser { public static void main(String[] args) { // Constants final int MAX_STORMS = 300;

Storm[] list = new Storm[MAX_STORMS];  // Creates array of Storms
Storm currentStorm;      		   // Storm returned by GetStorm
int nStorms = 0;        		   // Initializes number of storms in array List
int total = 0;          		   // Initializes total number of storms in the input file
Scanner fileInput;

// Opens the hurricane data file try{ System.out.println("Openning hurricane data file..."); fileInput = new Scanner(new File("hurricane.data")); } catch(FileNotFoundException e){ System.err.println("FileNotFoundException: " + e.getMessage()); return; } System.out.println( "File opened successfully..."); System.out.println( "Reading file..." );

// Reads Storm data from file until EOF (file)

while(fileInput.hasNextLine() ) { currentStorm = GetStorm(fileInput); total++; if( currentStorm.getCategory() >= 3 ) { list[nStorms] = currentStorm; nStorms++; } } System.out.println( "Number of storms: " + total); System.out.println( "Hurricanes with category 3 and above: " + nStorms ); DisplayStorms( "First Ten Storms", list, 10 ); Sort( list, nStorms ); DisplayStorms( "Top Twenty Storms", list, 20 ); fileInput.close(); }

public static Storm GetStorm( Scanner in ) { // Builds a Storm object and returns it

int year = 0, month = 0, day = 0, hour = 0;
    int sequence = 0, wind = 0, pressure = 0;
String name; 
int current = 0, beginDate = 0, duration = 0;
    int skip = 0;
    double junk = 0.0;
Storm newStorm; 

    		

// Reads the next record
year = in.nextInt();
    month = in.nextInt();
    day = in.nextInt();
    skip = in.nextInt();
    sequence = in.nextInt();
    name = in.next();
    junk = in.nextDouble();
    junk = in.nextDouble();
    wind = in.nextInt();
    pressure = in.nextInt();
    
            



// Makes a storm object and initializes it with info from the current record
beginDate = year * 10000 + month * 100 + day; // ex. 190600710
duration = 6;
newStorm = new Storm(beginDate, duration, name, wind, pressure);
current = sequence;


while( in.hasNextLine() && current == sequence ) 
{
	// Updates storm info
	duration += 6;
            newStorm.setDuration(duration);
            newStorm.setWind(wind);
            newStorm.setPressure(pressure);

  
	// Gets the next record 
	year = in.nextInt();
            month = in.nextInt();
            day = in.nextInt();
            skip = in.nextInt();
            sequence = in.nextInt();
            name = in.next();
            junk = in.nextDouble();
            junk = in.nextDouble();
            wind = in.nextInt();
            pressure = in.nextInt();
            
}

// Returns the new storm object return newStorm; }

public static void DisplayStorms( String title, Storm[] list, int nStorms ) { // Displays nStorms storms // Prints the title and column headings System.out.println(title + "\n"); System.out.println("Begin Date Duration Name Category Maximum Minimum"); System.out.println(" (hours) Winds (mph) Press. (mb)"); System.out.println("----------------------------------------------------------------"); for( int k = 0; k < nStorms; k++ ) System.out.print(list[k].toString()); System.out.println ("\n"); }

public static void Sort( Storm[] stormList, int n ) { // Bubble sorts the list of Storms int pass = 0, k, switches; Storm temp; switches = 1; while( switches != 0 ) { switches = 0; pass++; for( k = 0; k < n - pass; k++ ) { if( stormList[k].getCategory() < stormList[k+1].getCategory() ) { temp = stormList[k]; stormList[k] = stormList[k+1]; stormList[k+1] = temp; switches = 1; } } } } }

// Storm.java

public class Storm { private final double KnotsToMPH = 1.15;

// Global user-defined types: private int beginDate; private int duration; private String name; private int category; private int wind; private int pressure;

public Storm( int bdate, int dur, String sname, int w, int p ) { beginDate = bdate; duration = dur; name = sname; setWind(w); pressure = p; SaffirSimpson();

}

// Sets duration of storm public void setDuration( int d ) { duration = d; }

// Sets windspeed to knots and updates public void setWind( int w ) { double temp = w * KnotsToMPH; if(temp > wind) wind = (int)temp; SaffirSimpson(); }

// Sets pressute and updates public void setPressure( int p ) { if (pressure == 0) pressure = p; if(p > 0 && p < pressure) pressure = p; SaffirSimpson();

}

public void SaffirSimpson() { // Computes the storm category, using the Saffir-Simpson scale if(pressure <= 920 && wind >= 156) { category = 5; // Category 5 } if(pressure > 920 && wind < 156) { category = 4; // Category 4 } if(pressure > 945 && wind < 113) { category = 3; // Category 3 } if(pressure > 965 && wind < 96) { category = 2; // Category 2 }
if(pressure > 980 && wind < 83) { category = 1; // Category 1 } if(wind < 64) { category = -1; // Tropical Storm }
if(wind < 34) { category = -2; // Tropical Depression } if(pressure == 0) { category = 0; // Missing pressure } }

public int getCategory() { return category; }

public String toString() { // Formats variables return String.format("%9d %8d %10s %4d %9d %10d\n", beginDate, duration, name, category, wind, pressure);

}

}
