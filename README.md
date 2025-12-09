# Smart Health Agent System

A multi-agent AI system that provides comprehensive health planning using AutoGen. The system calculates BMI, creates personalized meal plans, and generates workout schedules through a sequential conversation flow.

## 🎯 Overview

This project implements a three-agent sequential workflow that:
1. **Calculates Health Metrics** - BMI, BMI Category, and Body Fat Percentage
2. **Creates Meal Plans** - Personalized one-week diet plans based on health metrics and preferences
3. **Schedules Workouts** - Tailored fitness routines that complement the diet plan

## ✨ Features

- **BMI Calculator Agent**: Calculates health metrics from physical details
- **Diet Planner Agent**: Creates personalized meal plans based on BMI results and diet preferences
- **Workout Scheduler Agent**: Generates weekly workout schedules aligned with health goals
- **Sequential Multi-Agent Workflow**: Agents work together, passing information seamlessly
- **Rule-Based Functions**: Fast, deterministic meal and workout plan generation
- **LLM-Powered Orchestration**: Natural language understanding and intelligent decision-making

## 🏗️ Architecture

### Agents

1. **BMI Calculator Agent** (`bmi_tool`)
   - Calculates BMI, BMI Category, and Body Fat Percentage
   - Uses the `health_metrics` function for calculations
   - Inputs: height, weight, age, gender

2. **Diet Planner Agent** (`diet_planner`)
   - Creates personalized one-week meal plans
   - Uses the `generate_meal_plan` function
   - Considers: BMI results, age, gender, diet preference (veg/vegan/non-veg)
   - Outputs: Daily meals, caloric targets, macronutrient breakdown

3. **Workout Scheduler Agent** (`workout_scheduler`)
   - Generates weekly fitness schedules
   - Uses the `generate_workout_plan` function
   - Considers: Meal plan suggestions, age, gender, fitness goals
   - Outputs: 7-day workout plan with cardio, strength, flexibility, and rest days

### Workflow

```
User Input (height, weight, age, gender, diet_preference)
    ↓
[BMI Calculator Agent] → Health Metrics
    ↓
[Diet Planner Agent] → Meal Plan (uses BMI results)
    ↓
[Workout Scheduler Agent] → Fitness Schedule (uses meal plan + user info)
    ↓
Complete Health Plan
```

## 📋 Prerequisites

- Python 3.8 or higher
- OpenAI API key
- pip package manager

## 🚀 Installation

1. **Clone the repository** (or navigate to the project directory):
   ```bash
   cd Scenario_iq
   ```

2. **Install dependencies**:
   ```bash
   pip install pyautogen langchain-community python-dotenv requests beautifulsoup4 ipython
   ```

3. **Create a `.env` file** in the project root:
   ```env
   OPENAI_API_KEY=your_openai_api_key_here
   ```

## ⚙️ Configuration

### LLM Configuration

The system uses GPT-4o-mini by default. You can modify the model in `smart_agent.py`:

```python
llm_config = {
    "config_list": [{"model": "gpt-4o-mini", "api_key": os.environ["OPENAI_API_KEY"]}],
}
```

### User Inputs

Modify the user inputs at the bottom of `smart_agent.py`:

```python
user_height = 186  # cm
user_weight = 98   # kg
user_age = 30
user_gender = "male"
diet_preference = "non-veg"  # "veg", "vegan", or "non-veg"
```

## 📖 Usage

### Basic Usage

1. **Set your user inputs** in `smart_agent.py` (lines ~327-332)

2. **Run the script**:
   ```bash
   python smart_agent.py
   ```

3. **View the results**: The system will output:
   - BMI calculation results
   - One-week meal plan
   - One-week workout schedule

### Example Input

```python
user_height = 186  # cm
user_weight = 98   # kg
user_age = 30
user_gender = "male"
diet_preference = "non-veg"
```

### Example Output

```
Complete Health Plan Workflow Finished!
============================================================

Chat 1 (BMI Calculation): BMI: 28.33, Category: Overweight, Body Fat: 24.69%

Chat 2 (Diet Plan): Weekly meal plan with breakfast, lunch, dinner, snacks
Daily calories: 1800 kcal
Macronutrients: Protein 30%, Carbs 40%, Fats 30%

Chat 3 (Workout Schedule): 7-day workout plan with cardio, strength, flexibility
Workout intensity: Moderate
Rest days: 2 per week
```

## 🔧 How It Works

### Agent Communication

The agents communicate through **sequential chat patterns**:

1. **BMI Agent** receives user input and calculates health metrics
2. **Diet Planner** receives BMI results (via conversation summary) + user info
3. **Workout Scheduler** receives meal plan summary + user info

### Function Registration

Each agent has registered functions:
- `health_metrics()` - Rule-based BMI calculation
- `generate_meal_plan()` - Rule-based meal plan generation
- `generate_workout_plan()` - Rule-based workout schedule generation

The LLM agents orchestrate when and how to call these functions, while the functions themselves execute deterministic logic.

## 📦 Dependencies

```
pyautogen
langchain-community
python-dotenv
requests
beautifulsoup4
ipython
```

## 🎨 Features Breakdown

### BMI Calculator
- Calculates BMI using standard formula
- Categorizes: Underweight, Normal, Overweight, Obese
- Estimates Body Fat Percentage using Deurenberg formula

### Diet Planner
- Supports: Vegetarian, Vegan, Non-vegetarian diets
- Adjusts calories based on BMI category
- Provides macronutrient breakdown
- Generates 7-day meal plans with variety

### Workout Scheduler
- Adapts intensity based on age
- Adjusts workout types based on fitness goals
- Includes: Cardio, Strength Training, Flexibility, Rest Days
- Provides age and gender-specific recommendations

## 🔍 Key Concepts

### LLM vs Rule-Based

- **LLM Agents**: Understand natural language, extract information, make decisions
- **Rule-Based Functions**: Fast, deterministic, consistent calculations
- **Combination**: LLM orchestrates, functions execute

### Sequential Pattern

The `autogen.initiate_chats()` function enables sequential agent communication:
- Each agent receives summaries from previous conversations
- Information flows naturally between agents
- Final output is a complete, integrated health plan

## 🐛 Troubleshooting

### Common Issues

1. **API Key Error**
   - Ensure `.env` file exists with `OPENAI_API_KEY`
   - Check that the key is valid and has credits

2. **Function Not Found**
   - Verify all functions are registered with `register_function()`
   - Check that agents have `register_for_execution()` calls

3. **Parameter Extraction Issues**
   - The LLM agents extract parameters from natural language
   - Ensure system messages clearly instruct parameter extraction
   - Check that previous conversation summaries contain needed information

## 📝 File Structure

```
Scenario_iq/
├── smart_agent.py      # Main script with all agents and functions
├── .env                # Environment variables (API keys)
└── README.md           # This file
```

## 🔮 Future Enhancements

- [ ] Add more meal variety and options
- [ ] Include calorie tracking
- [ ] Add progress monitoring
- [ ] Support for multiple languages
- [ ] Integration with fitness trackers
- [ ] Web interface
- [ ] Database for meal/workout history

## 📄 License

This project is provided as-is for educational and demonstration purposes.

## 👥 Credits

Built using:
- [AutoGen](https://github.com/microsoft/autogen) - Multi-agent conversation framework
- [OpenAI GPT-4o-mini](https://openai.com/) - LLM for agent orchestration

## 🤝 Contributing

Feel free to submit issues, fork the repository, and create pull requests for any improvements.

## 📧 Support

For questions or issues, please open an issue in the repository.

---

**Note**: This is a demonstration project. For actual health advice, consult with healthcare professionals.

