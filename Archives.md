#### AMAZON

1. https://leetcode.com/discuss/interview-question/6272497/AMAZON-or-L5-or-BLR-or-Onsite
    #### HLD to design health monitoring system for million services
      Design:
      <img width="785" alt="image" src="https://github.com/user-attachments/assets/f11c1cb8-83e6-41be-a1eb-c58acbd5ee88" />

      Tech:
      <img width="785" alt="image" src="https://github.com/user-attachments/assets/b3bc243e-b27a-4055-b4fe-3a90de3dbe26" />

     Link : https://gongybable.medium.com/system-design-design-a-monitoring-system-f0f0cbafc895
     ![image](https://github.com/user-attachments/assets/3d8cd3e9-c206-4018-929d-7f3475cba604)

     Exporter — Pulls metrics from targets and convert them to correct format\
     Push Gateway — Kron jobs to push metrics to at exit, then we can pull metrics from it\
     Data retrival workers — pull data\
     Time series storage — Local SSD / Remote Storage\
     Query Service — visualize data\
     Alert manager — to send alerts to different channels\
     Service Discovery — Configuration for the targets to pull metrics from\

```
  How to Collect Metrics — Pull or Push
There are two models to collect data, push and pull. In monitoring system, I would always go with pull model, and the reason is as below:

Scalability Concern. Our infrastructure will keep growing, and we many have hundreds or thousands of services in the coming years. And our service usage, user base will grow too. If we go with the push model, then all these services will keep hitting our monitor service. If we have a service which processes 1M requests per second, and this service push the metrics to our monitoring service upon every request, then we will suffer from scalability issue frequently as we grow. So instead of getting called to get metrics, I would prefer to actively pull the data from the services.
Automatic Upness Monitoring — By pulling the data proactively, we can directly know if the service is alive or not. For example, if one service is not reachable, we can be aware of it immediately.
Easier Horizontal Monitoring — If we have two independent systems A and B, but one day we need to monitor some service in system B from system A. We can pull metrics from system B directly, no need to configure system B to push to system A.
Easier for Testing — We can simply spin up testing env, and copy the configuration from production, then you can pull the same metrics as prod and do testing.
Simpler High Availability — just spin up two servers with the same configuration to pull the same data to achieve HA.
Less configuration, no need to configure every service.
Base on the analysis above, my design for the pull model is below:

Our service will pull the data from the services regularly (for example every second). We need a real time monitoring system, but a lag of a couple of seconds is totally fine.
Exporters — The services should not call our monitor service to send the data. Instead, they can save the metrics to an exporter, and the data can be stored there to get pulled. So that, our monitor service will not be exhausted from getting called, and it will be more scalable. Also our monitoring system may need the data in a specific format, and the services may be designed in different technologies, and have data in different formats. So we require an exporter attached to each service, which reformats the data into the correct format for our monitor services. And our monitor will pull the data from the exporters.
Push Gateway — For cron jobs, they are not service based, but we may need to monitor the metrics from them too. So we can have a push gateway, which lives behind all the cron jobs, and the monitor can just pull the data from the gateway directly.
Exporter Design
Since we discussed the components for the Pull model, i.e., Exporter, and Push Gateway.

Some interview may question why not have multiple services hooked to one exporter. And I would always prefer one service per exporter, and the argument is below:

Operational bottleneck — the exporter will become a bottleneck if we have too many services behind it
Single point of failure, and one service pushes too much will block others
If I am only interested in the metrics of one service, I cannot get that only, I have to read all
No upness monitoring — if one service is not reachable, we will not be able to know.
Hard to get service metadata — we can store the service metadata in the exporter

```
```
   Pull:
Use Case: When services are in a controlled network or support service discovery.
Example: Prometheus scraping Kubernetes pods.
Best For:
Systems with dynamic targets.
Centralized control over metrics collection.
Low-overhead metric retrieval.
Push:
Use Case: When services are in different networks, or you need real-time updates.
Example: IoT devices pushing metrics to a central system.
Best For:
Systems with firewalls/NAT.
Real-time applications needing low-latency updates.
Scenarios where service discovery is difficult.
```



     

   
2. Parking lot 
     Design :
     ![WhatsApp Image 2025-02-02 at 9 48 00 PM](https://github.com/user-attachments/assets/052f3247-dd8c-4801-8e41-f844d6485b38)  
     ```
       public class Vehicle {
    private String id; // Vehicle ID (e.g., license plate)
    private String type; // Vehicle type: "2W", "4W", "Heavy"
    private String owner; // Owner's name

    public Vehicle(String id, String type, String owner) {
        this.id = id;
        this.type = type;
        this.owner = owner;
    }

    public String getId() {
        return id;
    }

    public String getType() {
        return type;
    }

    public String getOwner() {
        return owner;
    }
    }

     ```
    ```
      public class ParkingSlot {
    private String id; // Slot ID
    private boolean isEmpty; // Status of the slot
    private double price; // Hourly rate for the slot
    private Vehicle vehicle; // Assigned vehicle (if any)

    public ParkingSlot(String id, double price) {
        this.id = id;
        this.price = price;
        this.isEmpty = true;
        this.vehicle = null;
    }

    public boolean isEmpty() {
        return isEmpty;
    }

    public void bookSlot(Vehicle vehicle) {
        if (!isEmpty) {
            throw new IllegalStateException("Slot is already occupied!");
        }
        this.vehicle = vehicle;
        this.isEmpty = false;
    }

    public void freeSlot() {
        this.vehicle = null;
        this.isEmpty = true;
    }

    public Vehicle getVehicle() {
        return vehicle;
    }

    public String getId() {
        return id;
    }

    public double getPrice() {
        return price;
    }
   }

    ```
    ```
         import java.time.LocalDateTime;

    public class Ticket {
    private String id; // Ticket ID
    private Vehicle vehicle; // Vehicle associated with the ticket
    private ParkingSlot parkingSlot; // Slot where the vehicle is parked
    private LocalDateTime entryTime; // Entry time of the vehicle

    public Ticket(String id, Vehicle vehicle, ParkingSlot parkingSlot) {
        this.id = id;
        this.vehicle = vehicle;
        this.parkingSlot = parkingSlot;
        this.entryTime = LocalDateTime.now();
    }

    public String getId() {
        return id;
    }

    public Vehicle getVehicle() {
        return vehicle;
    }

    public ParkingSlot getParkingSlot() {
        return parkingSlot;
    }

    public LocalDateTime getEntryTime() {
        return entryTime;
    }
   }

    ```
    ```
      public class ParkingSlotManager {
    private TwoWheelerParkingManager twoWheelerManager;
    private FourWheelerParkingManager fourWheelerManager;

    public ParkingSlotManager() {
        this.twoWheelerManager = new TwoWheelerParkingManager();
        this.fourWheelerManager = new FourWheelerParkingManager();
    }

    public void addTwoWheelerSlot(ParkingSlot slot) {
        twoWheelerManager.addSlot(slot);
    }

    public void addFourWheelerSlot(ParkingSlot slot) {
        fourWheelerManager.addSlot(slot);
    }

    public ParkingSlot findAvailableSlot(String vehicleType) {
        if (vehicleType.equals("2W")) {
            return twoWheelerManager.findAvailableSlot();
        } else if (vehicleType.equals("4W")) {
            return fourWheelerManager.findAvailableSlot();
        }
        return null; // No slots available for this type
    }

    public void bookSlot(ParkingSlot slot, Vehicle vehicle) {
        if (vehicle.getType().equals("2W")) {
            twoWheelerManager.bookSlot(slot, vehicle);
        } else if (vehicle.getType().equals("4W")) {
            fourWheelerManager.bookSlot(slot, vehicle);
        }
    }

    public void freeSlot(ParkingSlot slot, String vehicleType) {
        if (vehicleType.equals("2W")) {
            twoWheelerManager.freeSlot(slot);
        } else if (vehicleType.equals("4W")) {
            fourWheelerManager.freeSlot(slot);
        }
    }
   }


    ```
    ```
      import java.util.ArrayList;
    import java.util.List;

    public class TwoWheelerParkingManager {
    private List<ParkingSlot> twoWheelerSlots;

    public TwoWheelerParkingManager() {
        this.twoWheelerSlots = new ArrayList<>();
    }

    public void addSlot(ParkingSlot slot) {
        twoWheelerSlots.add(slot);
    }

    public ParkingSlot findAvailableSlot() {
        for (ParkingSlot slot : twoWheelerSlots) {
            if (slot.isEmpty()) {
                return slot;
            }
        }
        return null; // No available slot
    }

    public void bookSlot(ParkingSlot slot, Vehicle vehicle) {
        slot.bookSlot(vehicle);
    }

    public void freeSlot(ParkingSlot slot) {
        slot.freeSlot();
    }
   }

    ```
    ```
       import java.util.UUID;

    public class EntranceGate {
    private ParkingSlotManager parkingSlotManager;

    public EntranceGate(ParkingSlotManager parkingSlotManager) {
        this.parkingSlotManager = parkingSlotManager;
    }

    public Ticket generateTicket(Vehicle vehicle) {
        ParkingSlot slot = parkingSlotManager.findAvailableSlot(vehicle.getType());

        if (slot == null) {
            throw new IllegalStateException("No parking slot available for vehicle type: " + vehicle.getType());
        }

        parkingSlotManager.bookSlot(slot, vehicle);
        return new Ticket(UUID.randomUUID().toString(), vehicle, slot);
    }
   }

    ```
    ```
       import java.time.Duration;

    public class ExitGate {
    private ParkingSlotManager parkingSlotManager;

    public ExitGate(ParkingSlotManager parkingSlotManager) {
        this.parkingSlotManager = parkingSlotManager;
    }

    public double processPayment(Ticket ticket) {
        ParkingSlot slot = ticket.getParkingSlot();
        long parkedHours = Duration.between(ticket.getEntryTime(), LocalDateTime.now()).toHours();
        double totalCost = parkedHours * slot.getPrice();

        parkingSlotManager.freeSlot(slot); // Free the parking slot
        return totalCost;
     }
     }

   ```
   ```
      public class ParkingSystem {
    public static void main(String[] args) {
        // Initialize ParkingSlotManager
        ParkingSlotManager manager = new ParkingSlotManager();

        // Add slots
        manager.addTwoWheelerSlot(new ParkingSlot("T1", 20)); // 2W Slot
        manager.addFourWheelerSlot(new ParkingSlot("F1", 50)); // 4W Slot

        // Entrance Gate
        EntranceGate entrance = new EntranceGate(manager);

        // Vehicle Example
        Vehicle bike = new Vehicle("V123", "2W", "John Doe");
        Ticket bikeTicket = entrance.generateTicket(bike);
        System.out.println("Bike Ticket: " + bikeTicket.getId() + ", Slot: " + bikeTicket.getParkingSlot().getId());

        // Vehicle Example 2
        Vehicle car = new Vehicle("V456", "4W", "Jane Smith");
        Ticket carTicket = entrance.generateTicket(car);
        System.out.println("Car Ticket: " + carTicket.getId() + ", Slot: " + carTicket.getParkingSlot().getId());

        // Exit Gate
        ExitGate exit = new ExitGate(manager);
        double bikePayment = exit.processPayment(bikeTicket);
        System.out.println("Bike Payment: $" + bikePayment);

        double carPayment = exit.processPayment(carTicket);
        System.out.println("Car Payment: $" + carPayment);
    }
    }

   ```
    
