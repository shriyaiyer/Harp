# Harp
Creates a harp whose controls are the keys of the keyboard

/*
 * Name: Shriya Iyer
 * Recitation: 214
 * PennKey: shriyai
 * Description:  This Harp class creates a program that, when a certain key in 
 * NOTE_MAPPING is pressed, a corresponding sound is played
 */

public class Harp {
    private static double concertN = 440.0;
    private static String NOTE_MAPPING = 
        "q2we4r5ty7u8i9op-[=zxdcfvgbnjmk,.;/' ";

    public static void main(String[] args) {
        // create 37 harpStrings in an array
        HarpString [] stringArray = new HarpString[NOTE_MAPPING.length()];
        for (int i = 0; i < stringArray.length; i++) {
            stringArray[i] = new HarpString(concertN *
                                            Math.pow(2, (i - 24.0) / 12.0));
        }
        // creating an infinite loop
        while (true) {
            // check if the user has typed a key; if so, process it  
            double sample = 0.0;
            int key = 1;
            if (PennDraw.hasNextKeyTyped()) {
                key = PennDraw.nextKeyTyped();  // which key was pressed?
            }
            if (NOTE_MAPPING.indexOf(key) != -1) { // making sure that user
                // is pressing a key that is within NOTE_MAPPING
                stringArray[NOTE_MAPPING.indexOf(key)].pluck();
            }
            
            // compute the combined sound of all harp strings
            for (int i = 0; i < stringArray.length; i++) {
                sample += stringArray[i].sample();
            // advance the simulation of each harp string by one step 
                stringArray[i].tic();
            }
            
            // play the sample on standard audio
            StdAudio.play(sample); 
        }
    }
}
