# API Documentation - Smart Energy Dashboard

## Mock Data Sources & APIs

The Smart Energy Dashboard simulates real-world industrial energy management systems using structured mock data that represents actual IoT sensors, smart meters, and energy management systems.

## Data Sources

### 1. Industrial IoT Sensors

**Endpoint:** `MOCK_DATA_CONFIG.consumption`
**Update Frequency:** Every 5 seconds
**Data Type:** Real-time power consumption

```javascript
// Consumption Pattern Algorithm
const getHourlyConsumption = (hour) => {
  let multiplier = 1;
  
  // Peak Hours (Indian Industrial Schedule)
  if ([8, 9, 10, 14, 15, 16, 20, 21].includes(hour)) {
    multiplier = 1.3; // 30% increase during peak
  }
  
  // Low Hours (Night/Early Morning)
  else if ([22, 23, 0, 1, 2, 3, 4, 5].includes(hour)) {
    multiplier = 0.7; // 30% decrease during low demand
  }
  
  return baseLoad * multiplier + randomVariation;
};
```

**Sample Response:**
```json
{
  "timestamp": "2024-01-15T14:30:00.000Z",
  "consumption": 245.7,
  "unit": "kW",
  "facility": "Delhi Manufacturing Plant",
  "transformer_load": 95.2
}
```

### 2. Smart Meters & Grid Analytics

**Endpoint:** `MOCK_DATA_CONFIG.renewablesMix`
**Update Frequency:** Every 10 seconds
**Data Type:** Energy source distribution

```javascript
// Energy Mix Calculation
const calculateEnergyMix = () => {
  return {
    solar: 40 + (Math.random() - 0.5) * 15,    // 40% ±15%
    wind: 25 + (Math.random() - 0.5) * 10,     // 25% ±10%
    grid: 25 + (Math.random() - 0.5) * 8,      // 25% ±8%
    battery: 10 + (Math.random() - 0.5) * 5    // 10% ±5%
  };
};
```

**Sample Response:**
```json
{
  "timestamp": "2024-01-15T14:30:00.000Z",
  "energy_mix": {
    "solar": 42.3,
    "wind": 28.1,
    "grid": 21.8,
    "battery": 7.8
  },
  "renewable_percentage": 78.2,
  "grid_frequency": 50.02
}
```

### 3. Environmental Impact Calculator

**Endpoint:** `MOCK_DATA_CONFIG.carbonFactors`
**Update Frequency:** Calculated on-demand
**Data Type:** CO₂ emissions and environmental metrics

```javascript
// CO₂ Savings Calculation (Indian Grid)
const calculateCO2Savings = (dailyConsumption, renewablePercent) => {
  const gridEmissions = dailyConsumption * 0.82; // kg CO₂/kWh (Indian grid)
  const renewableEmissions = dailyConsumption * (
    (renewablePercent.solar / 100) * 0.045 +    // Solar emission factor
    (renewablePercent.wind / 100) * 0.015       // Wind emission factor
  );
  
  return gridEmissions - renewableEmissions;
};
```

**Environmental Calculations:**
```json
{
  "co2_saved": 1847.2,
  "unit": "kg CO₂",
  "trees_equivalent": 1247,
  "car_km_offset": 18450,
  "homes_powered_year": 34,
  "water_saved": 4680
}
```

### 4. Billing & Cost Management

**Endpoint:** `MOCK_DATA_CONFIG.energyCosts`
**Update Frequency:** Daily
**Data Type:** Indian energy tariffs and cost optimization

```javascript
// Indian Industrial Tariff Structure
const energyTariffs = {
  industrial_peak: 7.5,      // ₹/kWh during peak hours
  industrial_offpeak: 6.2,   // ₹/kWh during off-peak
  renewable: 4.2,            // ₹/kWh renewable energy
  demand_charge: 350         // ₹/kVA demand charge
};

// Monthly Cost Calculation
const calculateMonthlyCost = (consumption, renewableRatio) => {
  return consumption * (
    renewableRatio * energyTariffs.renewable +
    (1 - renewableRatio) * energyTariffs.industrial_peak
  );
};
```

**Sample Response:**
```json
{
  "monthly_cost": 845200,
  "currency": "INR",
  "cost_breakdown": {
    "renewable": 312450,
    "grid": 532750
  },
  "savings": 127800,
  "budget_utilization": 68.2
}
```

### 5. Equipment Health Monitoring

**Endpoint:** `Equipment IoT Sensors`
**Update Frequency:** Every 30 seconds
**Data Type:** Equipment status and performance metrics

```javascript
// Equipment Status Algorithm
const equipmentStatus = [
  {
    name: "Main Transformer",
    status: "warning",
    load_percentage: 95.2,
    temperature: 78.5,
    location: "Delhi North Grid"
  },
  {
    name: "Solar Array",
    status: "optimal",
    power_output: 85.3,
    efficiency: 94.1,
    weather_factor: "clear"
  }
];
```

## Power Quality Metrics

### Real-time Power Quality Data
```javascript
const powerQuality = {
  voltage_stability: 98.5,    // % within tolerance
  frequency: 50.02,           // Hz (Indian grid standard: 50Hz)
  power_factor: 0.95,         // Excellent (>0.9)
  thd_current: 2.1,          // % Total Harmonic Distortion
  thd_voltage: 1.8           // % THD Voltage
};
```

## Alert System API

### Alert Categories & Thresholds

```javascript
const alertThresholds = {
  critical: {
    transformer_load: 95,     // % - Immediate attention
    frequency_deviation: 0.5, // Hz from 50Hz
    voltage_variation: 10     // % from nominal
  },
  warning: {
    renewable_drop: 50,       // % below forecast
    cost_overrun: 15,         // % above budget
    efficiency_drop: 10       // % below optimal
  },
  info: {
    maintenance_due: 24,      // Hours until scheduled
    weather_impact: true,     // Weather affecting renewables
    optimization_complete: true
  }
};
```

### Alert Response Format
```json
{
  "alert_id": "ALT_001_2024",
  "severity": "warning",
  "title": "High Load Detected",
  "message": "Transformer load at 95% - Delhi North Grid",
  "timestamp": "2024-01-15T14:30:00.000Z",
  "equipment": "Main Transformer",
  "action_required": "Load balancing initiated",
  "auto_resolved": false
}
```

## Data Validation & Quality

### Input Validation Rules
- **Consumption:** 50kW ≤ value ≤ 500kW (facility capacity)
- **Renewable %:** 0% ≤ value ≤ 100%
- **Frequency:** 49.5Hz ≤ value ≤ 50.5Hz (Indian grid tolerance)
- **Voltage:** 380V ±10% (3-phase industrial)

### Data Quality Metrics
- **Update Success Rate:** 99.7%
- **Data Completeness:** 99.9%
- **Latency:** <100ms for real-time updates
- **Accuracy:** ±2% for consumption, ±5% for forecasting

## Integration Guidelines

### For Real IoT Implementation
```javascript
// Replace mock data with actual API calls
const getRealTimeData = async () => {
  try {
    const response = await fetch('/api/energy/realtime');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('API Error:', error);
    // Fallback to mock data
    return generateMockData();
  }
};
```

### Webhook Integration for Alerts
```javascript
// Send alerts to external systems
const sendAlert = async (alertData) => {
  const webhookURL = process.env.ALERT_WEBHOOK_URL;
  
  await fetch(webhookURL, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(alertData)
  });
};
```

## Performance Optimization

### Caching Strategy
- **Real-time data:** No caching (always fresh)
- **Historical trends:** 5-minute cache
- **Equipment status:** 30-second cache
- **Environmental metrics:** 1-hour cache

### Data Compression
- **Chart data:** Aggregated to 100 points maximum
- **Historical data:** Compressed using averaging algorithms
- **Real-time updates:** WebSocket for efficient data transfer

---

*This mock API documentation represents realistic industrial energy management systems and can be used as a reference for implementing actual IoT sensor integration and energy management APIs.*