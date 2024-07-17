Problem 1: Real-Time Weather Monitoring System
Here's the pseudocode, implementation, and documentation for the weather data fetching program:

### Pseudocode

1. Define constants for the API URL and API key.
2. Define a function `fetch_weather_data(location)` to:
   - Build the request URL.
   - Send a GET request to the API.
   - Parse the JSON response.
   - Check for errors in the response.
   - Extract relevant weather data (temperature, weather conditions, humidity, wind speed).
   - Return the extracted data or an error message.
3. Define a function `display_weather_data(weather_data)` to:
   - Print the weather data in a readable format.
4. Define a main function `main()` to:
   - Get the location input from the user.
   - Fetch the weather data for the input location.
   - Check for errors and display the weather data or an error message.
5. Call the main function if the script is run as the main module.

### Implementation

Here is the Python implementation of the weather data fetching program:

```python
import requests

# Constants for the weather API
API_URL = "http://api.openweathermap.org/data/2.5/weather"
API_KEY = "8a2870c4a35cccb57876eb7609adc86a"

def fetch_weather_data(location):
    """
    Fetches weather data for a given location using the OpenWeatherMap API.

    Args:
        location (str): The location for which to fetch weather data. Can be a city name or coordinates (lat,lon).

    Returns:
        dict: A dictionary containing the extracted weather data if successful.
        str: An error message if the request fails.
    """
    # Build the request URL
    request_url = f"{API_URL}?q={location}&appid={API_KEY}&units=metric"

    # Send the request to the API
    response = requests.get(request_url)

    # Parse the JSON response
    weather_data = response.json()

    if response.status_code != 200:
        return None, weather_data.get("message", "Failed to fetch weather data")

    # Extract relevant weather data
    temperature = weather_data["main"]["temp"]
    weather_conditions = weather_data["weather"][0]["description"]
    humidity = weather_data["main"]["humidity"]
    wind_speed = weather_data["wind"]["speed"]

    return {
        "temperature": temperature,
        "weather_conditions": weather_conditions,
        "humidity": humidity,
        "wind_speed": wind_speed
    }, None

def display_weather_data(weather_data):
    """
    Displays the fetched weather data in a readable format.

    Args:
        weather_data (dict): The dictionary containing the extracted weather data.
    """
    print(f"Temperature: {weather_data['temperature']}°C")
    print(f"Weather Conditions: {weather_data['weather_conditions']}")
    print(f"Humidity: {weather_data['humidity']}%")
    print(f"Wind Speed: {weather_data['wind_speed']} m/s")

def main():
    """
    The main function that drives the program.
    Gets the location input from the user, fetches the weather data,
    and displays it or an error message if the request fails.
    """
    # Get the location input from the user
    location = input("Enter the city name or coordinates (lat,lon): ")

    # Fetch the weather data for the input location
    weather_data, error = fetch_weather_data(location)

    if error:
        print(f"Error: {error}")
    else:
        # Display the fetched weather data
        display_weather_data(weather_data)

if __name__ == "__main__":
    main()
```

### Documentation

#### `fetch_weather_data(location)`
Fetches weather data for a given location using the OpenWeatherMap API.

**Args:**
- `location` (str): The location for which to fetch weather data. Can be a city name or coordinates (lat,lon).

**Returns:**
- `dict`: A dictionary containing the extracted weather data if successful.
- `str`: An error message if the request fails.

#### `display_weather_data(weather_data)`
Displays the fetched weather data in a readable format.

**Args:**
- `weather_data` (dict): The dictionary containing the extracted weather data.

#### `main()`
The main function that drives the program.
Gets the location input from the user, fetches the weather data, and displays it or an error message if the request fails.

**Args:**
- None

**Returns:**
- None




Problem 2: Inventory Management System Optimization
### Pseudocode

1. **Define the `Product` Class:**
   - Initialize with attributes: `id`, `name`, `category`, `cost`, `price`, `reorder_threshold`, `stock`.
   - Define a `__str__` method to return a string representation of the product.

2. **Define the `Warehouse` Class:**
   - Initialize with attributes: `id`, `name`, `capacity`, `inventory`.
   - Define methods to:
     - `add_product(product, quantity)`: Add or update product quantity in inventory.
     - `remove_product(product, quantity)`: Remove or reduce product quantity from inventory.
     - `check_stock(product)`: Return the stock level of a product.

3. **Initialize Data:**
   - Create instances of `Product` and `Warehouse`.
   - Add initial stock to warehouses.
   - Return lists of products and warehouses.

4. **Define Inventory Management Functions:**
   - `check_reorder(product, warehouse)`: Print reorder alert if stock is below threshold.
   - `generate_report(products, warehouses)`: Print stock levels for all products in all warehouses.

5. **Define the Main Function:**
   - Initialize data.
   - Enter a loop to display menu and handle user choices:
     - Check stock levels.
     - Generate reorder alerts.
     - Generate inventory report.
     - Exit program.

6. **Run the Main Function:**
   - If the script is run as the main module, call the `main` function.

### Implementation

Here is the Python implementation of the inventory management system:

```python
import datetime

# Define Product and Warehouse classes
class Product:
    def __init__(self, id, name, category, cost, price, reorder_threshold):
        self.id = id
        self.name = name
        self.category = category
        self.cost = cost
        self.price = price
        self.reorder_threshold = reorder_threshold
        self.stock = 0  # Current stock level

    def __str__(self):
        return f"{self.name} ({self.id}), Category: {self.category}, Price: ${self.price}, Stock: {self.stock}"

class Warehouse:
    def __init__(self, id, name, capacity):
        self.id = id
        self.name = name
        self.capacity = capacity
        self.inventory = {}  # Dictionary to store Product:quantity pairs

    def add_product(self, product, quantity):
        if product in self.inventory:
            self.inventory[product] += quantity
        else:
            self.inventory[product] = quantity

    def remove_product(self, product, quantity):
        if product in self.inventory and self.inventory[product] >= quantity:
            self.inventory[product] -= quantity
        else:
            print(f"Not enough stock of {product.name} in {self.name}")

    def check_stock(self, product):
        return self.inventory.get(product, 0)

# Sample data initialization
def initialize_data():
    # Create products
    product1 = Product(1, "Laptop", "Electronics", 800, 1200, 10)
    product2 = Product(2, "Smartphone", "Electronics", 500, 800, 15)

    # Create warehouses
    warehouse1 = Warehouse(1, "Main Warehouse", 1000)
    warehouse2 = Warehouse(2, "Secondary Warehouse", 500)

    # Add initial stock to warehouses
    warehouse1.add_product(product1, 50)
    warehouse1.add_product(product2, 100)
    warehouse2.add_product(product1, 20)

    return [product1, product2], [warehouse1, warehouse2]

# Inventory management functions
def check_reorder(product, warehouse):
    if warehouse.check_stock(product) <= product.reorder_threshold:
        print(f"Alert: {product.name} in {warehouse.name} needs to be reordered!")

def generate_report(products, warehouses):
    print("\n---- Inventory Report ----")
    for warehouse in warehouses:
        print(f"\nWarehouse: {warehouse.name}")
        for product in products:
            stock_level = warehouse.check_stock(product)
            print(f"{product.name}: Stock - {stock_level}")

def main():
    products, warehouses = initialize_data()

    while True:
        print("\n===== Inventory Management System =====")
        print("1. Check Stock")
        print("2. Generate Reorder Alerts")
        print("3. Generate Inventory Report")
        print("4. Exit")

        choice = input("Enter your choice: ")

        if choice == '1':
            for product in products:
                print(product)

        elif choice == '2':
            for warehouse in warehouses:
                for product in products:
                    check_reorder(product, warehouse)

        elif choice == '3':
            generate_report(products, warehouses)

        elif choice == '4':
            print("Exiting the program...")
            break

        else:
            print("Invalid choice. Please enter a valid option.")

if __name__ == "__main__":
    main()
```

### Documentation

#### `Product` Class

**Attributes:**
- `id` (int): Unique identifier for the product.
- `name` (str): Name of the product.
- `category` (str): Category of the product.
- `cost` (float): Cost price of the product.
- `price` (float): Selling price of the product.
- `reorder_threshold` (int): Stock level at which the product needs to be reordered.
- `stock` (int): Current stock level of the product (default is 0).

**Methods:**
- `__str__()`: Returns a string representation of the product.

#### `Warehouse` Class

**Attributes:**
- `id` (int): Unique identifier for the warehouse.
- `name` (str): Name of the warehouse.
- `capacity` (int): Maximum storage capacity of the warehouse.
- `inventory` (dict): Dictionary to store product-quantity pairs.

**Methods:**
- `add_product(product, quantity)`: Adds or updates the quantity of a product in the warehouse inventory.
- `remove_product(product, quantity)`: Removes or reduces the quantity of a product from the warehouse inventory.
- `check_stock(product)`: Returns the current stock level of a product in the warehouse.

#### `initialize_data()`

**Description:**
Initializes sample data by creating product and warehouse instances and adding initial stock to the warehouses.

**Returns:**
- `products` (list): List of product instances.
- `warehouses` (list): List of warehouse instances.

#### `check_reorder(product, warehouse)`

**Description:**
Checks if the stock level of a product in a warehouse is below the reorder threshold and prints an alert if necessary.

**Args:**
- `product` (Product): The product to check.
- `warehouse` (Warehouse): The warehouse to check.

#### `generate_report(products, warehouses)`

**Description:**
Generates and prints an inventory report showing the stock levels of all products in all warehouses.

**Args:**
- `products` (list): List of product instances.
- `warehouses` (list): List of warehouse instances.

#### `main()`

**Description:**
The main function that drives the inventory management system. Displays a menu to the user and handles choices for checking stock, generating reorder alerts, and generating inventory reports.

**Args:**
- None

**Returns:**
- None




Problem 3: Real-Time Traffic Monitoring System
### Pseudocode

1. **Define `get_geocode(location)` function:**
   - Build the geocode request URL using the provided location and API key.
   - Send a GET request to the OpenRouteService geocode API.
   - If the response status is not 200, raise an exception with the error message.
   - Parse the response JSON to extract coordinates.
   - If no results are found, raise an exception.
   - Return the coordinates of the location.

2. **Define `get_traffic_data(start, end)` function:**
   - Get geocodes for the start and end locations using the `get_geocode` function.
   - Handle exceptions and return an error message if geocoding fails.
   - Build the routing request URL.
   - Define headers and request body for the routing request.
   - Send a POST request to the OpenRouteService routing API.
   - If the response status is not 200, return an error message.
   - Parse the response JSON to extract route data.
   - If no routes are found, return an error message.
   - Extract relevant traffic information (distance, duration, polyline) from the route data.
   - Return the traffic information.

3. **Define `display_traffic_info(start, end)` function:**
   - Call `get_traffic_data(start, end)` to get traffic information.
   - If an error is returned, print the error message.
   - If successful, print the traffic information (start address, end address, distance, duration, polyline).

4. **Main Execution:**
   - Define hardcoded start and end locations.
   - Call `display_traffic_info(start, end)` to display traffic information.

### Implementation

Here is the Python implementation of the traffic data fetching program:

```python
import requests

# Replace with your OpenRouteService API key
ORS_API_KEY = '5b3ce3597851110001cf62486bc5f40875af473089f7932ac5265372'

def get_geocode(location):
    """
    Fetches the geocode (latitude and longitude) for a given location.

    Args:
        location (str): The location to geocode.

    Returns:
        list: Coordinates [longitude, latitude] of the location.

    Raises:
        Exception: If the geocode fetch fails or no results are found.
    """
    geocode_url = f"https://api.openrouteservice.org/geocode/search?api_key={ORS_API_KEY}&text={location}"
    response = requests.get(geocode_url)

    if response.status_code != 200:
        raise Exception(f"Error fetching geocode: {response.status_code}")

    data = response.json()
    if 'features' not in data or len(data['features']) == 0:
        raise Exception("No geocoding results found.")

    coords = data['features'][0]['geometry']['coordinates']
    return coords

def get_traffic_data(start, end):
    """
    Fetches traffic data for a route between two locations.

    Args:
        start (str): The starting location.
        end (str): The ending location.

    Returns:
        dict: A dictionary containing traffic information including distance, duration, and polyline.
              Returns an error message in case of failure.
    """
    try:
        start_coords = get_geocode(start)
        end_coords = get_geocode(end)
    except Exception as e:
        return {'error': str(e)}

    routing_url = f"https://api.openrouteservice.org/v2/directions/driving-car"
    headers = {
        'Authorization': ORS_API_KEY,
        'Content-Type': 'application/json'
    }
    body = {
        "coordinates": [start_coords, end_coords]
    }

    response = requests.post(routing_url, json=body, headers=headers)

    if response.status_code != 200:
        return {'error': f"Error fetching route: {response.status_code}"}

    route_data = response.json()

    if 'routes' not in route_data:
        return {'error': 'No routes found'}

    route = route_data['routes'][0]
    summary = route['summary']

    traffic_info = {
        'start_address': start,
        'end_address': end,
        'distance': summary['distance'] / 1000,  # convert to km
        'duration': summary['duration'] / 60,  # convert to minutes
        'polyline': route['geometry']
    }

    return traffic_info

def display_traffic_info(start, end):
    """
    Displays traffic information for a route between two locations.

    Args:
        start (str): The starting location.
        end (str): The ending location.
    """
    traffic_info = get_traffic_data(start, end)
    if 'error' in traffic_info:
        print("Error:", traffic_info['error'])
    else:
        print("Traffic Information:")
        print(f"Start Address: {traffic_info['start_address']}")
        print(f"End Address: {traffic_info['end_address']}")
        print(f"Distance: {traffic_info['distance']} km")
        print(f"Duration: {traffic_info['duration']} minutes")
        print("Polyline:", traffic_info['polyline'])

# Hardcoded start and end locations
start = "New York, NY"
end = "Los Angeles, CA"
display_traffic_info(start, end)
```

### Documentation

#### `get_geocode(location)`

**Description:**
Fetches the geocode (latitude and longitude) for a given location.

**Args:**
- `location` (str): The location to geocode.

**Returns:**
- `list`: Coordinates [longitude, latitude] of the location.

**Raises:**
- `Exception`: If the geocode fetch fails or no results are found.

#### `get_traffic_data(start, end)`

**Description:**
Fetches traffic data for a route between two locations.

**Args:**
- `start` (str): The starting location.
- `end` (str): The ending location.

**Returns:**
- `dict`: A dictionary containing traffic information including distance, duration, and polyline.
- Returns an error message in case of failure.

#### `display_traffic_info(start, end)`

**Description:**
Displays traffic information for a route between two locations.

**Args:**
- `start` (str): The starting location.
- `end` (str): The ending location.



Problem 4: Real-Time COVID-19 Statistics Tracker
### Pseudocode

1. **Define `get_covid_stats(region)` function:**
   - Set the base URL for the COVID-19 API.
   - If the region is "world", set the URL to fetch global statistics.
   - Otherwise, set the URL to fetch statistics for the specified country.
   - Send a GET request to the API.
   - If the response status is not 200, print an error message and return `None`.
   - Parse the response JSON data and return it.

2. **Define `display_covid_stats(region)` function:**
   - Call `get_covid_stats(region)` to get COVID-19 statistics.
   - If the stats are `None`, print a failure message and return.
   - If the stats contain an error message, print the error message and return.
   - Print the COVID-19 statistics for the specified region, including total cases, deaths, recovered cases, active cases, and new cases, deaths, and recoveries today if available.

3. **Main Execution:**
   - Define a hardcoded region for demonstration purposes.
   - Call `display_covid_stats(region)` to display COVID-19 statistics for the specified region.

### Implementation

Here is the Python implementation for fetching and displaying COVID-19 statistics:

```python
import requests

def get_covid_stats(region):
    """
    Fetches COVID-19 statistics for the specified region.

    Args:
        region (str): The region to fetch statistics for (country name or "world").

    Returns:
        dict: A dictionary containing COVID-19 statistics for the region.
              Returns None if data retrieval fails.
    """
    base_url = "https://disease.sh/v3/covid-19"
    if region.lower() == "world":
        url = f"{base_url}/all"
    else:
        url = f"{base_url}/countries/{region}"

    response = requests.get(url)

    if response.status_code != 200:
        print(f"Error fetching data: {response.status_code}")
        print(response.text)  # Print the response text for debugging
        return None

    data = response.json()
    return data

def display_covid_stats(region):
    """
    Displays COVID-19 statistics for the specified region.

    Args:
        region (str): The region to display statistics for (country name or "world").
    """
    stats = get_covid_stats(region)

    if stats is None:
        print("Failed to retrieve data.")
        return

    if 'message' in stats:
        print(f"Error: {stats['message']}")
        return

    print(f"COVID-19 Statistics for {region.capitalize()}:")
    print(f"Total Cases: {stats.get('cases', 'N/A')}")
    print(f"Total Deaths: {stats.get('deaths', 'N/A')}")
    print(f"Total Recovered: {stats.get('recovered', 'N/A')}")
    print(f"Active Cases: {stats.get('active', 'N/A')}")
    if 'todayCases' in stats:
        print(f"New Cases Today: {stats.get('todayCases', 'N/A')}")
    if 'todayDeaths' in stats:
        print(f"New Deaths Today: {stats.get('todayDeaths', 'N/A')}")
    if 'todayRecovered' in stats:
        print(f"New Recoveries Today: {stats.get('todayRecovered', 'N/A')}")

# Hardcoded region for demonstration purposes
region = "China"  # Change this to any country name or "world" for global stats
display_covid_stats(region)
```

### Documentation

#### `get_covid_stats(region)`

**Description:**
Fetches COVID-19 statistics for the specified region.

**Args:**
- `region` (str): The region to fetch statistics for (country name or "world").

**Returns:**
- `dict`: A dictionary containing COVID-19 statistics for the region.
- Returns `None` if data retrieval fails.

**Raises:**
- Prints an error message if the API request fails.

#### `display_covid_stats(region)`

**Description:**
Displays COVID-19 statistics for the specified region.

**Args:**
- `region` (str): The region to display statistics for (country name or "world").

**Raises:**
- Prints an error message if the statistics contain an error message or if data retrieval fails.
