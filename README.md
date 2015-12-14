# Weather
Guide/Program to make a working Java program to get the weather forecast, using OpenWeatherMap & API.

Aside from json-jar library and your own API link, all in eclipse programmable.
Getting API Link: get from OpenWeatherMap Homepage by registering there, free version is an option.
Also, further guides are available on homepage of OpenWeatherMap.

In the following a couple of different uses will be listed:


import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.URL;
import java.nio.charset.Charset;

import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Locale;

import java.util.Scanner;

import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

public class DownloadUrl {

	private static String readAll(Reader rd) throws IOException {
		StringBuilder sb = new StringBuilder();
		int cp;
		while ((cp = rd.read()) != -1) {
			sb.append((char) cp);
		}
		return sb.toString();
	}

	public static JSONObject readJsonFromUrl(String url) throws IOException,
			JSONException {
		InputStream is = new URL(url).openStream();
		try {
			BufferedReader rd = new BufferedReader(new InputStreamReader(is,
					Charset.forName("UTF-8")));
			String jsonText = readAll(rd);
			JSONObject json = new JSONObject(jsonText);
			return json;
		} finally {
			is.close();
		}
	}
	

	public static void main(String[] args) throws IOException, JSONException {

		System.out
				.println("Today's Temperature: Please choose 1 for Zuerich, 2 for London, 3 for Paris, 4 for Rome or 5 for Boston.");
		System.out
				.println("7 days forecast: Please choose 11 for Zuerich, 22 for London, 33 for Paris, 44 for Rome or 55 for Boston.");
		System.out
		    .println("Special options: 0 & -1");

		Scanner scanner = new Scanner(System.in);
		int t = scanner.nextInt();
		scanner.close();

		int a = 1;
		int d = 0;

		JSONObject json = null;
		JSONObject json_specific = null; // get specific data in jsonobject variable
		Double result_temp = null; // get integer/double of temperature variable

		JSONArray json_list = null; // get array list of jsonarray variable
		JSONObject json_specific_day = null; // pick specific day variable out of list

		JSONObject json_city = null; //get city out of list
		String json_city_name = null; //get string of the prior picked city
		
		SimpleDateFormat df1 = new SimpleDateFormat("EEE", Locale.ENGLISH); //For only 3 initials of the day
		SimpleDateFormat df2 = new SimpleDateFormat("EEEE", Locale.ENGLISH); //Entire word/day as output
		Calendar c = Calendar.getInstance();
		
//example 1: One city & today's weather, case scenario
    
		if (t < 6 && t > 0) {
			switch (t) {
			
			//the link in the following is listed multiple times except for the small difference of the desired city
			//in order: zuerich, london, paris, roma, boston
			
			case 1:
				json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/weather?q=zuerich&mode=json&units=metric&cnt=7&appid=
				add_your_API_code_here");
				break;
			case 2:
				json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/weather?q=london&mode=json&units=metric&cnt=7&appid=
				add_your_API_code_here");
				break;
			case 3:
				json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/weather?q=paris&mode=json&units=metric&cnt=7&appid=
				add_your_API_code_here");
				break;
			case 4:
				json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/weather?q=roma&mode=json&units=metric&cnt=7&appid=
				add_your_API_code_here");
				break;
			case 5:
				json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/weather?q=boston&mode=json&units=metric&cnt=7&appid=
				add_your_API_code_here");
				break;
			default:
				System.out
						.println("Please restart the program and be sure to choose one of the possible requests.");
			}
			json_specific = json.getJSONObject("main");
			result_temp = json_specific.getDouble("temp");
			c.add(Calendar.DATE, d);
			String output = df2.format(c.getTime());
			
			System.out.println("\n"+"Today is "+ output+ " and the Temperature in "+json.get("name")+" is: "+result_temp);
			
// Example 2: Display of whole week of a specific city
			
		} else if (t > 10 && t < 56) {
			switch (t) {
			case 11:
				json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=Zuerich&mode=json&units=metric&cnt=7&appid=
				add_your_API_code_here");
				break;
			case 22:
				json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=London&mode=json&units=metric&cnt=7&appid=
				add_your_API_code_here");
				break;
			case 33:
				json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=Paris&mode=json&units=metric&cnt=7&appid=
				add_your_API_code_here");
				break;
			case 44:
				json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=Roma&mode=json&units=metric&cnt=7&appid=
				add_your_API_code_here");
				break;
			case 55:
				json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=Boston&mode=json&units=metric&cnt=7&appid=
				add_your_API_code_here");
				break;
			default:
				System.out
						.println("Please restart the program and be sure to choose one of the possible requests.");
			}
			json_list = json.getJSONArray("list");
			json_city = json.getJSONObject("city");
			json_city_name = json_city.getString("name");
			System.out.println("\n" + "The Forecast for " + json_city_name + " is: " + "\n");
			for (int i = 0; i < 7; i++) {
				json_specific_day = json_list.getJSONObject(i);
				json_specific = json_specific_day.getJSONObject("temp");
				result_temp = json_specific.getDouble("day");
				
				// The following are the remnants of the output of the list I had to work through to get the desired data
				
				// System.out.println("The Complete List is " + json_list);
				// System.out.println("List of Weather of " + a + ". Day is " + json_specific_day);
				// System.out.println("Whole Temp Data of " + a + ". Day is " + json_specific);
				
				//  Finally the desired forecast of the week of a specific city
				
				System.out.println("The Temperature of the " + a + ". Day is " + result_temp);
				a++;
			}

// Example 3: Forecast day & night for all listed cities and their week

		} else if (t == 0) {

			for (int i = 1; i < 6; i++) {
				switch (i) {
				case 1:
					json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=Zuerich&mode=json&units=metric&cnt=7&appid=
					add_your_API_code_here");
					break;
				case 2:
					json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=London&mode=json&units=metric&cnt=7&appid=
					add_your_API_code_here");
					break;
				case 3:
					json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=Paris&mode=json&units=metric&cnt=7&appid=
					add_your_API_code_here");
					break;
				case 4:
					json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=Roma&mode=json&units=metric&cnt=7&appid=
					add_your_API_code_here");
					break;
				case 5:
					json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=Boston&mode=json&units=metric&cnt=7&appid=
					add_your_API_code_here");
					break;
				}
				json_list = json.getJSONArray("list");
				json_city = json.getJSONObject("city");
				json_city_name = json_city.getString("name");
				
				System.out.println("\n" + "The Forecast for " + json_city_name + " is: " + "\n");
				
				for (int j = 0; j < 7; j++) {
				
					Double result_temp2 = null;

					json_specific_day = json_list.getJSONObject(j);
					json_specific = json_specific_day.getJSONObject("temp");
					result_temp = json_specific.getDouble("day");
					result_temp2 = json_specific.getDouble("night");

					System.out.println(a + ". Day it's " + result_temp + " and at Night it's " + result_temp2+".");
					a++;
				}
				a = 1;
				
				//following is a note of the list from which of the data "day" & "night" was picked
				// "temp":{"min":-2.78,"eve":-2.78,"max":-1.4,"morn":-2.78,"night":-1.4,"day":-2.78}}
			}
			
// Example 4: Basically example 3, but with weekdays initials included.

		}else if (t==-1){
			for (int i = 1; i < 6; i++) {
				switch (i) {
				case 1:
					json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=Zuerich&mode=json&units=metric&cnt=7&appid=
					add_your_API_code_here");
					break;
				case 2:
					json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=London&mode=json&units=metric&cnt=7&appid=
					add_your_API_code_here");
					break;
				case 3:
					json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=Paris&mode=json&units=metric&cnt=7&appid=
					add_your_API_code_here");
					break;
				case 4:
					json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=Roma&mode=json&units=metric&cnt=7&appid=
					add_your_API_code_here");
					break;
				case 5:
					json = readJsonFromUrl("http://api.openweathermap.org/data/2.5/forecast/daily?q=Boston&mode=json&units=metric&cnt=7&appid=
					add_your_API_code_here");
					break;
				}
				json_list = json.getJSONArray("list");
				json_city = json.getJSONObject("city");
				json_city_name = json_city.getString("name");
				System.out.println("\n" + "The Forecast for " + json_city_name + " is: " + "\n");
				
				for (int j = 0; j < 7; j++) {
				
					Double result_temp2 = null;
					
					json_specific_day = json_list.getJSONObject(j);
					json_specific = json_specific_day.getJSONObject("temp");
					result_temp = json_specific.getDouble("day");
					result_temp2 = json_specific.getDouble("night");
					
					c.add(Calendar.DATE, d);
					String output = df1.format(c.getTime());
					
					System.out.println(output + ". it's " + result_temp + " and at Night it's " + result_temp2+".");
					d++;
				}
				
			}
		}
		
	}
}
