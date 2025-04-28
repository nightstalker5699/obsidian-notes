### **PostgreSQL Timezone & Timestamp Handling**

PostgreSQL provides **two main timestamp types** and supports **time zone conversions** efficiently.
to summarize timestamp store the time provided while time zone give it the offset in your current time zone 

---

## **1. Timestamp Data Types**

|Data Type|Description|Example|
|---|---|---|
|`TIMESTAMP WITHOUT TIME ZONE`|Stores date & time **without** time zone info.|`'2024-03-18 14:00:00'`|
|`TIMESTAMP WITH TIME ZONE` (`TIMESTAMPTZ`)|Stores date & time **with** automatic time zone conversion.|`'2024-03-18 14:00:00+00'`|

### ✅ **Example: Creating a Table**

```sql
CREATE TABLE events (
    event_id SERIAL PRIMARY KEY,
    event_time TIMESTAMP WITH TIME ZONE
);
```

---

## **2. Getting Current Time with Time Zone**

### ✅ **Check Current Timestamp**

```sql
SELECT NOW();  -- Default: TIMESTAMPTZ
```

✔ Output: `2024-03-18 14:30:00+00`

### ✅ **Get Current Time in UTC**

```sql
SELECT CURRENT_TIMESTAMP AT TIME ZONE 'UTC';
```

✔ Output: `2024-03-18 14:30:00`

---

## **3. Setting and Checking Time Zone**

### ✅ **Check Current Time Zone**

```sql
SHOW TIMEZONE;
```

✔ Example Output: `UTC` (Default)

### ✅ **Set Time Zone for Current Session**

```sql
SET TIMEZONE = 'America/New_York';
```

### ✅ **Get Time in a Different Time Zone**

```sql
SELECT NOW() AT TIME ZONE 'Asia/Tokyo';
```

✔ Converts to **Tokyo time**.

---

## **4. Converting Time Zones**

### ✅ **Convert a Stored Timestamp to Another Time Zone**

```sql
SELECT event_time AT TIME ZONE 'America/Los_Angeles' FROM events;
```

---

## **5. Casting & Formatting Timestamps**

### ✅ **Convert String to Timestamp**

```sql
SELECT '2024-03-18 14:00:00+00'::TIMESTAMPTZ;
```

### ✅ **Format Timestamp in ISO 8601**

```sql
SELECT TO_CHAR(NOW(), 'YYYY-MM-DD"T"HH24:MI:SSOF');
```

✔ Output: `2024-03-18T14:30:00+00:00`

---

### **Key Takeaways**

- ✅ `TIMESTAMPTZ` auto-adjusts time zones.
- ✅ Use `SHOW TIMEZONE;` to check, and `SET TIMEZONE = 'UTC';` to change.
- ✅ `AT TIME ZONE 'X'` converts timestamps.
- ✅ PostgreSQL **defaults to ISO 8601** for timestamps.

Would you like a **real-world example** using events and scheduling? 🚀