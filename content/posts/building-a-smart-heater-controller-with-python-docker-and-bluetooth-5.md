---
title: "Building a Smart Heater Controller with Python, Docker, and Bluetooth #5"
date: 2025-01-10
---

# Chapter 5: Exposing the Python Script Through an API to Control Heaters From Home Assistant

In this chapter, I’ll walk you through how I exposed my Python script via an API, allowing it to be controlled directly from Home Assistant (HA). We’ll detail the steps taken to build the API, integrate it with HA, and expand functionality with Zigbee USB devices in a Dockerized environment.

**😺 Python git project**  
Feel free to fork and improve ! Git hub

**🎥 HA Overview video**  
![HA Overview video](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fvwqr8e73ffgm6hovi8ze.jpg)

#### Section 1: Building the API for the Python Script

To enable control of the heaters, I implemented a RESTful API with the following:

- **Framework Selection**: I used Flask to create the API due to its simplicity and compatibility with the existing Python script.
    
- **API Endpoint**:
    
    - **POST /set-temp**: This endpoint allows setting the temperature of a heater. The payload includes the room name and the target temperature, e.g., `{"room": "kitchen", "temp": 22}`.
    

- **Dockerizing the API**:
    
    - The Flask app was containerized using Docker.
    - The `Dockerfile` was configured to expose port 5000 for the API.
    - Used `docker run` to grant access to the network and expose the API locally:
    
    ```
        docker run -d --net=host --privileged -v /var/run/dbus:/var/run/dbus --name heater-controller
    ```
    

- **Testing the API**:
    
    - Used Postman and `curl` to test the `/set-temp` endpoint.
    

```
curl --location 'http://raspberrypi.local:5000/set-temp' 
--header 'Content-Type: application/json' 
--data '{
    "temperature": 60,
    "room":"living_room"
}'
```

- Ensured proper error handling for invalid room names or temperatures outside acceptable ranges.

#### Section 2: Plugging Home Assistant Into the Local API

With the API running, I connected it to HA to enable heater control:

- **Configuration in HA**:
    
    - Edited `configuration.yaml` to create REST commands for the API:
    
    ```
    rest_command:
      set_heater_temp:
        url: "http://localhost:5000/set-temp"
        method: POST
        headers:
          Content-Type: application/json
        payload: '{"room": "{{ room }}", "temp": {{ temp | float }}}'
    ```
    

- **Common Errors and Fixes**:
    
    - **Floats Sent as Strings**: Adjusted templates in HA to ensure temperatures were sent as numbers.
    - **Timeout Issues**: Increased the API timeout in HA to handle slower network responses.
    

#### Section 3: Controlling Heaters From Home Assistant

After integrating the API, I added controls in HA to manage heaters effectively:

- **Helpers**:
    
    - Set up `input_number` helpers for each room to adjust target temperatures.
    

- **Automation Scripts**:
    
    - Created scripts to send temperature updates to the API when a helper value changes.
    
    ```
    - id: update_heater_temp
      alias: Update Heater Temperature
      trigger:
        - platform: state
          entity_id: input_number.kitchen_temp
      action:
        - service: rest_command.set_heater_temp
          data:
            room: "kitchen"
            temp: "{{ states('input_number.kitchen_temp') | float }}"
    ```
    

- **UI Enhancements**:
    
    - Configured a Lovelace dashboard to show sliders for each room’s temperature control.
    - Included feedback on current heater temperatures retrieved via sensors.
    

#### Section 4: Using Zigbee in a Dockerized HA Instance

I extended the system by adding Zigbee functionality for further automation:

- **USB Port Exposure**:
    
    - Updated Docker Compose to pass through the Zigbee USB coordinator:
    
    ```
    devices:
      - "/dev/ttyUSB0:/dev/ttyUSB0"
    ```
    

- **Zigbee2MQTT Integration**:
    
    - Installed Zigbee2MQTT to manage Zigbee devices using a Conbee II USB coordinator.
    - Added Zigbee sensors (e.g., window sensors) to HA.
    

- **Automation with Zigbee Sensors**:
    
    - Linked Zigbee sensors to heater control via automation scripts.
    - Example: Turn off a room’s heater when its window sensor reports it is open.
    

### Final Thoughts

By exposing the Python script as an API and integrating it with HA, I created a reliable system for controlling heaters. The addition of Zigbee functionality further enhanced the system, enabling seamless automation. This setup demonstrates the flexibility and scalability achievable with Docker, APIs, and smart home integrations.

Go to Source
