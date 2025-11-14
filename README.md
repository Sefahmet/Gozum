# Smart Navigation System for the Visually Impaired: **Gözüm**

## Project Name Suggestion: **Gözüm**

**Meaning:** The word "Gözüm" in Turkish means both "my eye" (suggesting a personal guide) and "my precious/valuable." This directly relates to the guidance and value a navigation application provides to visually impaired users. It is a short, memorable, and Turkish name.

## Project Introduction

This project is a **location-based smart navigation system** developed to enable visually impaired individuals to move safely and independently in indoor and outdoor environments. Built with a Java-based **Spring Boot** backend and the **OpenLayers** mapping library, the system calculates the shortest path between the user's current location and the destination. It then provides real-time **audio feedback** about important structures or buildings along this route.

## Key Features

| Feature | Description | Technology/Component |
| :--- | :--- | :--- |
| **Shortest Path Calculation** | Calculates the shortest and optimized route between the start and end points. | `ShortestPathService`, Dijkstra/A* Algorithm (estimated) |
| **Audio Feedback** | Announces information about important points (buildings, structures) as the user approaches them on the route. | JavaScript `SpeechSynthesisUtterance` (Text-to-Speech) |
| **Geographic Data Processing** | Processes map data (building, road networks) and performs transformations between geographic coordinate systems (Lat/Lon ↔ EN). | `org.locationtech.jts.geom`, Shapefile (SHP) Data |
| **RESTful API** | Provides clean and standardized API endpoints for easy integration with mobile or web clients. | Spring Boot `@RestController` |
| **Map Visualization** | Visualizes the calculated route and geographic data on a map. | OpenLayers |

## Technologies Used

### Backend
*   **Java 17+**
*   **Spring Boot:** Application framework and dependency management.
*   **Maven:** Project build and compilation tool.
*   **Lombok:** To reduce boilerplate code.
*   **JTS (Java Topology Suite):** For geographic data processing and coordinate transformations.

### Frontend
*   **HTML, CSS, JavaScript**
*   **OpenLayers:** Mapping visualization library.
*   **Web Speech API:** For audio feedback (Text-to-Speech).

## Setup and Running

### Prerequisites

*   Java Development Kit (JDK) 17 or higher
*   Maven
*   Git

### Steps

1.  **Clone the Project:**
    ```bash
    git clone https://github.com/Sefahmet/LBS_project.git
    cd LBS_project
    ```

2.  **Load Dependencies and Build the Project:**
    ```bash
    mvn clean install
    ```

3.  **Run the Application:**
    ```bash
    mvn spring-boot:run
    ```
    The application will start running at `http://localhost:8080` by default.

4.  **Data Set Check:**
    Ensure that the Shapefile (SHP) data (e.g., `nodes.shp`, `edges.shp`, `Buildings.shp`, etc.), which contains the road network and building information, is present in the project's `src/main/resources/Data` directory. This data is critical for navigation calculations.

## API Endpoints

The application provides the following RESTful API endpoints for core functionalities:

| Method | Endpoint | Description | Parameters |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/shortestPath/shortest-path-params` | Calculates the shortest path between two points. | `lat1`, `lon1`, `lat2`, `lon2` (double) |
| `GET` | `/api/building/infoLatLon` | Returns information about the nearest building to a point on the specified route, used for audio feedback. | `lat`, `lon` (double), `pathId` (UUID) |

## Usage Scenario (Frontend Interaction)

1.  The user selects the start and end points on the map.
2.  The frontend sends a request to the `/api/shortestPath/shortest-path-params` endpoint using these coordinates.
3.  The backend calculates the shortest path and returns the route coordinates along with a unique `pathId` (route ID).
4.  The frontend draws the route on the map and stores the `pathId`.
5.  When the user clicks on a point along the route, the frontend sends a request to the `/api/building/infoLatLon` endpoint using the coordinates of that point and the stored `pathId`.
6.  The backend returns the information of the nearest building to that point on the user's route.
7.  The frontend reads the returned text information aloud to the user via the `speak()` function (Text-to-Speech).

## Development Notes

*   **Coordinate System Transformation:** The application converts geographic coordinates (WGS84 - Lat/Lon) to a local Euclidean coordinate system (EN) for processing map data. This is essential for the accuracy of distance and path calculations.
*   **Data Storage:** The calculated route coordinates are temporarily stored in the `RouteDataStorage` singleton class for building information queries.
*   **Security:** The application has a basic security configuration with `WebSecurityConfig.java`, but security auto-configuration (`SecurityAutoConfiguration`) is disabled in `LbsProjectApplication.java`. A detailed review of the security layer is recommended before deploying the project.
