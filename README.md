# AI_3
Code for AI_3 of the overall GS1 Specifications Project

This file is part of an unfinished project for a Software Engineering class at Jacksonville State University.
In this project, we must create a program that can take a barcode from an input source, convert that into a string, 
  find individual codes within that string, sort through all of them, and print out what each code represents, all 
  according to the GS1 specifications for barcodes.
From the main class of the program, the individual codes are sorted by the first character and then sent out to their 
  respective AI classes.
If the first character happens to be a three, it is sent through this file.
In this file, pattern matchers are set up...

          super.matchers.put( "30", Pattern.compile("^30\\d{1,8}[%\\x1D]") );
          super.matchers.put( "p30eol", Pattern.compile("^30\\d{1,8}$") );
          super.matchers.put( "310n", Pattern.compile("^310[0-9]{7}") );
          super.matchers.put( "p311n", Pattern.compile("^311[0-9]{7}") );
          continued...
          
  ...that will go from that first character and look at the next one to three characters.
  
I'll be using this codes for the following explanations: 

          super.matchers.put( "30", Pattern.compile("^30\\d{1,8}[%\\x1D]") );
          super.matchers.put( "p30eol", Pattern.compile("^30\\d{1,8}$") );
          
The ^30 in the code represents the AI that we may be looking for. That being the first character followed by any other 
  characters that may fit the pattern. So if the code were "3042354659", it would locate the first two characters which 
  are "3" and "0".
The \\d in the code is just a regular expression. By using that, it will only locate digits (0, 1, 2, etc.) in the code. 
  So after it finds that "30" in the code, it will keep going, looking at eeach digit until it is told to stop.
The {1,8} in the code is what tells the \\d how many digits there may be. Because of the \\d, it will collect digits after 
  the "30" until it fills up those eight spaces or until a delimiter is reached. So if the code were "3042354659", it would 
  fill up with 42354659.
The [%\\x1D] or the $ are both delimiters. They are there so that the \\d would know when to stop in case there happened to 
  be less than eight digits. The GS1 specifications state that the delimiter for the code would either be a % or an x1D, so 
  this is what was used as a delimiter in this situation. The GS1 specifications also state that the % or x1D would not be 
  needed if the code is at the end of the string, which is why the $ is used, as it marks the end of a line. So if the code 
  were "3042354%", it would have no problem filling up the spaces with just the five digits, "42354".
  
I'll be using this code for the following explanations: 

          super.matchers.put( "310n", Pattern.compile("^310[0-9]{7}") );
          
The [0-9] acts the same as the \\d, searching for digits from zero to nine.
The {7} is similar to the previous {1,8}, but in this case, there are exactly seven spaces and they all need to be filled up.
  So if the code were "31046537", the spaces would fill up with "46537", and then, with that not being enough digits to fill 
  the spaces, the program would return an error and cut itself off.
A delimiter is not needed in this case as the code is [0-9] is shown exactly where to cut off and there can be no fewer digits 
  to make it stop sooner.
  
As for the code is located beneath these pattern matchers...

          public Object parse30(String element) {
              HashMap<String, Object> output = new HashMap<>();
              System.out.println("Found Element String: " + element);
              output.put("title", "VAR. COUNT");
              output.put("ai", element.substring(0, 2) );
              output.put("count", Integer.parseInt( element.substring(2) ));
              output.put("element", element);
              return output;
          }

  ...these are what parse the codes that are found into smaller sections, with each having a specific meaning.
  
The above code will be used for the following explanations.
The first item that is printed out will be what characters were found. This would be the full list of characters found, such 
  as the string "3042354659" that was used earlier.
The next item that is printed out will be the title for that AI, which is found in the list of GS1 Specifications. For AI 30, 
  the title is declared to be "VAR. COUNT".
The third item to be printed out is the AI, which would be the "30" that was found using the pattern matchers. In this case, 
  it takes the characters at the places of 0 and 1 which are "3" and "0" respectively.
The fourth item to be printed out is the count, which starts at the place of 2 and continues until the end, as this string 
  does not have a defined end in this case. Using the string "3042354659", it would print out "42354659".
The fifth and final item to be printed out is the element once again, which was "3042354659".

