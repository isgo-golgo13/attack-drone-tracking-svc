# DoD Delta Attack Drone Tracking Service
Go 1.23 Attack Drone Tracking Service using React.js, Three.js and ScyllaDB NoSQL Drone Tracking DB


# Drone Tracking Service Architecture

IN-PROGRESS

# Drone Tracking Service Database Schema Design

IN-PROGRESS

```cql
-- Keyspace creation
CREATE KEYSPACE IF NOT EXISTS drone_tracking
WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 3};

-- Table for drone information
CREATE TABLE IF NOT EXISTS drone_tracking.drone_info (
    drone_id UUID PRIMARY KEY,
    model TEXT,
    serial_number TEXT,
    registration_date TIMESTAMP,
    active BOOLEAN,
    owner TEXT
);

-- Table for storing drone status (flight log)
CREATE TABLE IF NOT EXISTS drone_tracking.drone_status (
    drone_id UUID,
    status_id TIMEUUID,
    latitude DOUBLE,
    longitude DOUBLE,
    altitude DOUBLE,
    speed DOUBLE,
    battery_level INT,
    status TEXT,
    last_communication TIMESTAMP,
    PRIMARY KEY (drone_id, status_id)
) WITH CLUSTERING ORDER BY (status_id DESC);

-- Table for storing drone maintenance logs
CREATE TABLE IF NOT EXISTS drone_tracking.drone_maintenance (
    drone_id UUID,
    maintenance_id TIMEUUID,
    maintenance_date TIMESTAMP,
    maintenance_type TEXT,
    description TEXT,
    technician TEXT,
    PRIMARY KEY (drone_id, maintenance_id)
) WITH CLUSTERING ORDER BY (maintenance_id DESC);

-- Table for storing drone assignments
CREATE TABLE IF NOT EXISTS drone_tracking.drone_assignments (
    drone_id UUID,
    assignment_id TIMEUUID,
    mission_name TEXT,
    mission_location TEXT,
    assigned_date TIMESTAMP,
    status TEXT,
    PRIMARY KEY (drone_id, assignment_id)
) WITH CLUSTERING ORDER BY (assignment_id DESC);

```

To apply the CQL schema apply as follows.

```shell
cqlsh -f drone-tracking-db.cql
```
