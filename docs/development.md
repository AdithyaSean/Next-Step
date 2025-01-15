# Development Guide: Integrating Python ML Models with Spring Boot

This document provides detailed instructions on how to integrate the trained Python ML models (`trainer.py`, `preprocessor.py`, `predictor.py`, `generator.py`, and `config.py`) into a Spring Boot application for real-time career predictions.

## Overview

The Python ML pipeline consists of the following components:
- **Data Generation**: Synthetic student profiles are generated using `generator.py`.
- **Data Preprocessing**: The data is preprocessed using `preprocessor.py`.
- **Model Training**: The RandomForestRegressor model is trained using `trainer.py`.
- **Prediction**: The trained model is used to predict career probabilities using `predictor.py`.

The Spring Boot application will utilize the trained model (`career_predictor.joblib`) and metadata (`model_metadata.joblib`) to make predictions based on student input.

## Prerequisites

1. **Python Environment**: Ensure Python 3.12+ is installed with the required dependencies (`scikit-learn`, `pandas`, `numpy`, `joblib`).
2. **Spring Boot Application**: A Spring Boot application with REST API endpoints for handling student data and predictions.
3. **Model Files**: The trained model files (`career_predictor.joblib`, `model_metadata.joblib`, `feature_selector.joblib`, `scaler.joblib`) should be available in the `models` directory.

## Steps to Integrate ML Models with Spring Boot

### 1. Train and Export the Model

Run the following commands to generate the model files:

```bash
# Generate synthetic data
python -m main generate

# Preprocess the data
python -m main process

# Train the model
python -m main train
```

This will generate the following files in the `models` directory:
- `career_predictor.joblib`: The trained RandomForestRegressor model.
- `model_metadata.joblib`: Metadata containing feature names, career names, and best parameters.
- `feature_selector.joblib`: The feature selector used during training.
- `scaler.joblib`: The scaler used for preprocessing.

### 2. Create a Spring Boot Service for Predictions

Create a Spring Boot service that loads the trained model and makes predictions.

#### Example: `PredictionService.java`

```java
@Service
public class PredictionService {

    private RandomForestRegressor model;
    private Map<String, Integer> featureOrder;
    private List<String> featureNames;
    private List<String> careerNames;
    private StandardScaler scaler;

    @PostConstruct
    public void init() {
        try {
            // Load the trained model and metadata
            this.model = (RandomForestRegressor) joblib.load("models/career_predictor.joblib");
            Map<String, Object> metadata = (Map<String, Object>) joblib.load("models/model_metadata.joblib");
            this.featureNames = (List<String>) metadata.get("feature_names");
            this.careerNames = (List<String>) metadata.get("career_names");
            this.scaler = (StandardScaler) joblib.load("models/scaler.joblib");
            this.featureOrder = (Map<String, Integer>) joblib.load("models/feature_order.joblib");
        } catch (Exception e) {
            throw new RuntimeException("Failed to load model files", e);
        }
    }

    public Map<String, Double> predictCareerProbabilities(StudentProfile profile) {
        // Create a feature vector based on the student profile
        double[] features = new double[featureNames.size()];
        Arrays.fill(features, -1.0);  // Initialize with -1

        // Set basic features
        features[featureOrder.get("education_level")] = profile.getEducationLevel();
        features[featureOrder.get("AL_stream")] = profile.getAlStream();
        features[featureOrder.get("gpa")] = profile.getGpa();

        // Set OL subject scores
        for (Map.Entry<String, Double> entry : profile.getOlResults().entrySet()) {
            String colName = "OL_subject_" + entry.getKey() + "_score";
            if (featureOrder.containsKey(colName)) {
                features[featureOrder.get(colName)] = entry.getValue();
            }
        }

        // Set AL subject scores
        if (profile.getAlResults() != null) {
            for (Map.Entry<String, Double> entry : profile.getAlResults().entrySet()) {
                String colName = "AL_subject_" + entry.getKey() + "_score";
                if (featureOrder.containsKey(colName)) {
                    features[featureOrder.get(colName)] = entry.getValue();
                }
            }
        }

        // Scale the features
        double[] scaledFeatures = scaler.transform(new double[][]{features})[0];

        // Make prediction
        double[] predictions = model.predict(new double[][]{scaledFeatures})[0];

        // Return career probabilities
        Map<String, Double> careerProbabilities = new HashMap<>();
        for (int i = 0; i < careerNames.size(); i++) {
            careerProbabilities.put(careerNames.get(i), predictions[i] * 100);
        }

        return careerProbabilities;
    }
}
```

### 3. Create a REST Controller for Predictions

Create a REST controller to expose the prediction functionality.

#### Example: `PredictionController.java`

```java
@RestController
@RequestMapping("/predictions")
public class PredictionController {

    private final PredictionService predictionService;

    @Autowired
    public PredictionController(PredictionService predictionService) {
        this.predictionService = predictionService;
    }

    @PostMapping("/career")
    public Map<String, Double> predictCareer(@RequestBody StudentProfile profile) {
        return predictionService.predictCareerProbabilities(profile);
    }
}
```

### 4. Define the Student Profile Model

Define a `StudentProfile` class to represent the input data for predictions.

#### Example: `StudentProfile.java`

```java
public class StudentProfile {
    private int educationLevel;
    private int alStream;
    private double gpa;
    private Map<String, Double> olResults;
    private Map<String, Double> alResults;

    // Getters and setters
}
```

### 5. Test the Integration

Run the Spring Boot application and test the prediction endpoint using a tool like Postman or cURL.

#### Example Request:

```json
{
  "educationLevel": 1,
  "alStream": 0,
  "gpa": 3.75,
  "olResults": {
    "0": 85,
    "1": 78,
    "2": 72,
    "3": 65,
    "4": 70,
    "5": 75
  },
  "alResults": {
    "0": 88,
    "1": 82,
    "2": 90
  }
}
```

#### Example Response:

```json
{
  "Engineering": 85.3,
  "Medicine": 78.5,
  "IT": 72.1,
  "Business": 65.4,
  "Teaching": 70.2,
  "Research": 75.8
}
```

## Conclusion

By following these steps, you can integrate the Python-trained ML models into a Spring Boot application to provide real-time career predictions. This setup allows the Spring Boot application to leverage the power of the trained RandomForestRegressor model while maintaining a clean separation between the ML pipeline and the application logic.

For further improvements, consider:
- Adding model versioning and monitoring.
- Implementing caching for frequently requested predictions.
- Enhancing the API with additional features like batch predictions.