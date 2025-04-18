# exercise UML diagram 1

I configured 5 classes:
1. <<interface>> MedicalData (rest API)
2. PatientVitals
3. Alert
4. AlertGenerator
5. AlertManager

Class 1 is designed to function as an API interface, providing controlled access to medical data. It defines the contract (MedicalData) that is implemented by Class 2 (PatientVitals), which contains the actual private data fields. These private members are intentionally encapsulated to ensure that data can only be accessed via the MedicalData interface, thereby enforcing abstraction and access control.

The PatientVitals object, instantiated through the interface, is then passed to the Alert class. The Alert class evaluates the incoming data to determine whether the medical readings exceed critical thresholds. If so, it triggers the AlertGenerator.

The AlertGenerator class has a dependency on the Alert class, which allows it to access evaluation results and relevant data. Furthermore, AlertGenerator maintains a composition relationship with the AlertManager class. This indicates a strong lifecycle dependency: the AlertManager cannot exist independently of AlertGenerator, and its existence is tightly coupled to it. Conceptually, without an AlertManager, no actionable alarm or response can be executed.

This design pattern supports data protection and legal compliance by ensuring that sensitive client data is only accessible to authorized components (e.g., certified developers or medical personnel). The core data remains private, while evaluation and alert functionalities are modular and externally accessible, allowing for controlled interactions with non-privileged systems.

# exercise UML diagram 2

I configured 5 classes:
1. <<interface>> DataInterface (rest API)
2. DataStorage
3. PatientData
4. AccesControl
5. DataRetriever


The system is designed to protect patient data through strict encapsulation and access control. At the core, we define a DataInterface, which acts as an abstraction layer for all data storage and retrieval operations. The concrete implementation, DataStorage, inherits from this interface and ensures that all access to raw data (e.g. PatientData) passes through a controlled gateway.

This design ensures that no component in the system can bypass security checks or manipulate data directly. Instead, all components  such as DataRetriever (used by medical personnel for querying patient vitals)  depend on DataInterface to interact with the underlying data. This enforces separation of concerns: DataRetriever queries; DataStorage manages persistence and policy enforcement.

The PatientData class itself does not expose its internals arbitrarily. It relies on a dependency with AccessControl, a class responsible for evaluating whether a given User has the proper privileges to access or modify the data. The AccessControl class exposes only one public method, typically accesControl(userId), which evaluates access rights based on the user's role and authorized patient list. All other methods in AccessControl — such as modifying access roles or managing user permissions — are private or restricted to administrators, doctors, or system handlers.
# exercise UML diagram 3

I configured 4 classes:
1. IdentifyManager
2. SimulatorPatientIds
3. PatientIdentifier
4. PatientRecord



This system is architected with a hierarchical and dependency-driven structure. At its core, the IdentifyManager class maintains a composition relationship with PatientIdentifier, indicating that the functionality of patient identification is wholly dependent on the existence and control of the manager component. In other words, the lifecycle of PatientIdentifier is tightly bound to IdentifyManager, and cannot function independently.

Additionally, IdentifyManager exhibits a dependency relationship with SimulatorPatientIDs, as it must ingest and process simulated identifier data, particularly for the purpose of handling exceptional or edge-case conditions. This dependency is transient and reflects a usage-based interaction rather than structural ownership.

The PatientIdentifier class, in turn, holds dependency relationships with both PatientRecord and SimulatorPatientIDs. These dependencies arise from the need to perform set-theoretic operations—specifically, identifying the intersection between real patient records and simulated identifiers. These operations enable the system to determine which individuals are common to both datasets, thereby facilitating accurate patient matching in simulation or testing contexts.
# exercise UML diagram 4

I configured 6 classes:
1. DataListener
2. TCPDataListener
3. Websocket
4. FileDataListener
5. DataParser
6. DataSourceAdapter



This system is designed to handle data coming from different sources, like TCP connections, WebSockets, or log files. To manage this, we created an interface called DataListener. This interface defines a method called listen() that each specific data source must use. Then we have three classes—TCPDataListener, WebSocketListener, and FileDataListener—that each implement DataListener and know how to handle their own type of input.

Each of these listeners sends the raw data they receive to a shared class called DataParser. This class is responsible for turning different formats, like JSON or CSV, into clean, usable data objects. That way, the listener classes don’t have to worry about how the data looks—they just pass it on to the parser.

Once the data is parsed, it goes to the DataSourceAdapter, which connects to the rest of the system. This adapter sends the cleaned data to wherever it needs to go next, like a database or a real-time monitoring tool.

We built the system this way so that each part does only one job. If we need to add a new data source in the future (like MQTT), we can just make a new class that also uses the DataListener interface. This keeps the system flexible, easier to update, and more reliable.
