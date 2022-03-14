# HarmanAssignment
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.Timer;
import java.util.TimerTask;

public class HarmanWrittenTest {

static LinkedHashMap<String, String> allCharacters = new LinkedHashMap<String, String>();

public static void main(String[] args) {

//calling apis every 10s
Timer timer = new Timer();
timer.scheduleAtFixedRate(new TimerTask() {

public void run() {
allCharacters = getLatestValues(allCharacters);
}
},0,10000);
}

    public static LinkedHashMap<String, String> getLatestValues(LinkedHashMap<String, String> allCharacters) {
   
    LinkedHashMap<String, String> allCharactersMap = new LinkedHashMap<String, String>();
   
    String uriAvengers = "http://www.mocky.io/v2/5ecfd5dc3200006200e3d64b";
    String uriHeroes =   "http://www.mocky.io/v2/5ecfd630320000f1aee3d64d";
    String urimutants =  "http://www.mocky.io/v2/5ecfd6473200009dc1e3d64e";
    String[] apiArray = {"uriAvengers","uriHeroes","urimutants"};
    for (int i=0; i<apiArray.length; i++)  
    {  
    allCharactersMap = getCharacters(apiArray[i], allCharactersMap);
    allCharacters = allCharactersMap;
   
    }  
    return allCharacters;
    }
   
    //Calling APIs and updating map
    public static LinkedHashMap<String, String> getCharacters(String uri,LinkedHashMap<String, String> allCharacters) {
   
    RestTempalate restTemplate = new RestTemplate();
    Object[] response = restTemplate.getForObject(uri,Object[].class);
    JSONObject obj = new JSONObject(response);
    JSONArray characters = obj.getJSONArray("character");
        JSONObject jObject = new JSONObject(characters);
        Iterator<?> mapkeys = jObject.keys();
        while( mapkeys.hasNext() ){
            String key = (String)mapkeys.next();
            String value = jObject.getString(key);
           
            // Removing 1st inserted character if count exceeds 15.
            if(allCharacters.size()>15) {
            allCharacters.removeEldestEntry(mapkeys.keys.first());
            }
            allCharacters.put(key, value);

        }  
    return allCharacters;  
    }
   
    //This method retuns the power of character. Actually We need to write restcontroller for this method.
    public static String characterPower(String character) {
    return allCharacters.get(character);
    }
    }
