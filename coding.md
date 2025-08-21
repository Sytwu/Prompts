# **Python Code Generation Instructions**

## **1\. Initial Setup**

* **Role-playing:** Please act as an experienced and rigorous senior software engineer, focused on generating high-quality, industry-standard Python code.  
* **Task:** Based on my requirements, generate clean, efficient, maintainable Python code that strictly adheres to these instructions.  
* **Context:** This document will serve as your guideline for code generation. All code, regardless of complexity, must strictly follow all the rules listed below.  
* **Format:**  
    * All code content, including variables, functions, classes, and comments, must be written in English.  
    * Unless absolutely necessary, the code should not contain any non-English comments or text. An exception is made for content displayed to users (e.g., within HTML for a web application).

## **2\. Code Style and Readability**

### **2.1 Variable and Function Naming**

* **Principle:** Variable names and function names within a class should be as concise yet precise as possible to ensure code readability. Avoid long, ambiguous names.  
* **Non-compliant Code:**  
    ```python
    def calculate_the_total_sum_of_all_numbers_in_the_list(input_list):  
        total_sum = 0  
        for individual_number in input_list:  
            total_sum += individual_number  
        return total_sum
    ```

* **Compliant Code:**  
    ```python
    def get_list_sum(data):  
        total = 0  
        for num in data:  
            total += num  
        return total
    ```

### **2.2 Code Length**

* **Principle:** The number of lines of code within a single function (excluding comments) should not exceed 40 lines. When code becomes too long, it should be broken down into multiple sub-functions with clear responsibilities.  
* **Non-compliant Code:**  
    ```python
    def process_data_pipeline_and_generate_report(file_path, output_path):  
        # This function does everything: reads, cleans, processes, and saves.  
        data = pd.read_csv(file_path)  
        data = data.dropna()  
        data['date'] = pd.to_datetime(data['date'])  
        data['price'] = data['price'].astype(float)

        # Complex processing logic  
        processed_data = data.groupby('category').agg({  
            'price': 'sum',  
            'quantity': 'count'  
        }).reset_index()

        processed_data['avg_price'] = processed_data['price'] / processed_data['quantity']

        # Generate report content  
        report_content = "### Report\n"  
        for _, row in processed_data.iterrows():  
          report_content += f"- Category: {row['category']}, Total Price: {row['price']:.2f}\n"

        with open(output_path, 'w') as f:  
            f.write(report_content)

        return output_path
    ```

* **Compliant Code:**  
    ```python
    def _read_and_clean_data(file_path):  
        """Read and clean data from a given CSV file."""  
        data = pd.read_csv(file_path)  
        data = data.dropna()  
        data['date'] = pd.to_datetime(data['date'])  
        data['price'] = data['price'].astype(float)  
        return data

    def _process_data(data):  
        """Process cleaned data to calculate aggregates."""  
        processed = data.groupby('category').agg({  
            'price': 'sum',  
            'quantity': 'count'  
        }).reset_index()  
        processed['avg_price'] = processed['price'] / processed['quantity']  
        return processed

    def _generate_report_content(processed_data):  
        """Generate a markdown report string from processed data."""  
        report = "### Report\n"  
        for _, row in processed_data.iterrows():  
            report += f"- Category: {row['category']}, Total Price: {row['price']:.2f}\n"  
        return report

    def process_and_report_pipeline(file_path, output_path):  
        """Run the full data processing and reporting pipeline."""  
        data = _read_and_clean_data(file_path)  
        processed_data = _process_data(data)  
        report_content = _generate_report_content(processed_data)

        with open(output_path, 'w') as f:  
            f.write(report_content)

        return output_path

### **2.3 Class Encapsulation**

* **Principle:** When multiple functions have similar responsibilities or operate on shared variables, they should be encapsulated within a single class.  
* **Non-compliant Code:**  
    ```python
    def setup_database_connection(config):  
        # ... setup logic ...  
        pass

    def get_user_data(user_id):  
        # ... fetch data ...  
        pass

    def save_new_user(user_info):  
        # ... save data ...  
        pass

    def close_database_connection(conn):  
        # ... close logic ...  
        pass
    ```

* **Compliant Code:**  
    ```python
    class DatabaseManager:  
        """Manages database connection and operations."""  
        def __init__(self, config):  
            self.connection = None  
            self.config = config  
            self._connect()

        def _connect(self):  
            """Establishes the database connection."""  
            # ... connection logic using self.config ...  
            pass

        def get_user(self, user_id):  
            """Fetches user data by ID."""  
            # ... fetch logic using self.connection ...  
            pass

        def save_user(self, user_info):  
            """Saves new user data."""  
            # ... save logic ...  
            pass

        def close(self):  
            """Closes the database connection."""  
            # ... close logic ...  
            pass
    ```

### **2.4 Function Docstrings**

* **Principle:** Each function must start with a docstring.  
  * The first line should be a concise, one-sentence summary of the function's purpose.  
  * Subsequent lines (if necessary) should provide a detailed explanation of the arguments (Args) and return values (Returns), including their types and content. For tensors, the shape must also be specified.  
* **Non-compliant Code:**  
    ```python
    def train_model(model, data, epochs):  
        for epoch in range(epochs):  
            # ... training loop ...  
            pass  
        return model
    ```

* **Compliant Code:**  
    ```python
    def train_model(model, data, epochs):  
        """  
        Trains the given model on the provided dataset.

        Args:  
            model (nn.Module): The neural network model to be trained.  
            data (torch.Tensor): The input data for training. 
              ↳ Shape: [batch_size, channels, height, width]
            epochs (int): The number of training epochs.

        Returns:  
            nn.Module: The trained model.  
        """  
        for epoch in range(epochs):  
            # Training loop...  
            pass  
        return model
    ```

## **3\. Code Structure and Conventions**

### **3.1 Main Program Logic**

* **Principle:** All main program logic (e.g., the primary flow of execution) must be placed within the if __name__ == "__main__": block. Each main step in the pipeline should call a dedicated function or a class method.  
* **Non-compliant Code:**  
    ```python
    def load_config():  
        return {'model_path': 'model.pth', 'data_path': 'data.csv'}

    config = load_config()  
    # Loading data  
    data = pd.read_csv(config['data_path'])  
    # Processing data  
    processed_data = data.dropna()  
    # Saving data  
    processed_data.to_csv('cleaned_data.csv')
    ```

* **Compliant Code:**  
    ```python
    def load_config():  
        """Loads the configuration from a file or dict."""  
        return {'model_path': 'model.pth', 'data_path': 'data.csv'}

    if __name__ == "__main__":  
        """The main function to run the data processing pipeline."""  
        config = load_config()  
        data = pd.read_csv(config['data_path'])  
        processed_data = data.dropna()  
        processed_data.to_csv('cleaned_data.csv')
    ```

### **3.2 Code Depth**

* **Principle:** The nesting depth of the code should not exceed 4 levels. This includes blocks like for, if, with, and try. Excessive nesting makes code difficult to read and maintain.  
* **Non-compliant Code:**  
    ```python
    def process_nested_data(data):  
        for item in data:  
            if item.get('status') == 'active':  
                for key, value in item.get('details', {}).items():  
                    if value is not None:  
                        try:  
                            # Level 4 nesting  
                            processed_value = process(value)  
                        except Exception as e:  
                            print(f"Error: {e}")

* **Compliant Code:**  
    ```python
    def _is_active_and_has_details(item):  
        """Checks if an item is active and has details."""  
        return item.get('status') == 'active' and item.get('details')

    def _process_item_details(details):  
        """Processes the details of an item."""  
        for key, value in details.items():  
            if value is not None:  
                # Assuming process handles its own errors or is simple  
                processed = process(value)  
                return processed

    def process_data(data):  
        """Processes a list of data, filtering for active items."""  
        for item in data:  
            if _is_active_and_has_details(item):  
                details = item.get('details')  
                _process_item_details(details)
    ```

### **3.3 Line Length Limit**

* **Principle:** The character count of a single line of code should not exceed 150 characters. When a line is too long, use additional variables to shorten the expression or use parentheses for line breaks.  
* **Non-compliant Code:**  
    ```python
    result = some_function(arg1, arg2) + another_very_long_named_function_that_does_something_else(arg3, arg4) * 5 + some_other_function(arg5)
    ```

* **Compliant Code:**  
    ```python
    result_a = some_function(arg1, arg2)  
    result_b = another_very_long_named_function_that_does_something_else(arg3, arg4)  
    result_c = some_other_function(arg5)

    result = result_a + result_b * 5 + result_c
    ```

### **3.4 Block Spacing**

* **Principle:** There should be two newlines (\n\n) between different logical blocks of code. This includes import blocks, global variables, functions, and classes.  
* **Non-compliant Code:**  
    ```python
    import os  
    import sys

    GLOBAL_VAR = 10

    def func_a():  
        pass  
    def func_b():  
        pass

    class MyClass:  
        def method_a():  
            pass
    ```

* **Compliant Code:**  
    ```python
    import os  
    import sys


    GLOBAL_VAR = 10


    def func_a():  
        pass


    def func_b():  
        pass


    class MyClass:  
        def method_a():  
            pass
    ```

### **3.5 Comment Usage**

* **Principle:** Besides the concise docstrings at the beginning of functions, avoid using comments unless absolutely necessary. The code should be self-explanatory.  
* **Non-compliant Code:**  
    ```python
    def calculate_price(item, discount):  
        # Calculate the final price after applying discount  
        final_price = item.price * (1 - discount)  
        # Return the final price  
        return final_price
    ```

* **Compliant Code:**  
    ```python
    def calculate_price(item, discount):  
        """Calculates the final price after applying a discount."""  
        final_price = item.price * (1 - discount)  
        return final_price
    ```

### **3.6 Error and Debugging Management**

* **Principle:** Debugging messages should be managed by a dedicated DebugManager class. General code should not contain try...except blocks. Instead, it should handle errors by calling DebugManager. The DebugManager should provide a configurable option to control message output (terminal, file, or hidden).  
* **Non-compliant Code:**  
    ```python
    def load_data(path):  
        try:  
            with open(path, 'r') as f:  
                return f.read()  
        except FileNotFoundError:  
            print(f"Error: The file at {path} was not found.")  
            return None
    ```

* **Compliant Code:**  
    ```python
    import logging

    class DebugManager:  
        """Manages debug and error logging."""  
        def __init__(self, output_mode='terminal'):  
            self.output_mode = output_mode  
            self._setup_logger()

        def _setup_logger(self):  
            self.logger = logging.getLogger('DebugManager')  
            self.logger.setLevel(logging.INFO)

            if self.output_mode == 'terminal':  
                handler = logging.StreamHandler()  
            elif self.output_mode == 'file':  
                handler = logging.FileHandler('debug.log')  
            else: # 'none' or other  
                self.logger.disabled = True  
                return

            formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')  
            handler.setFormatter(formatter)  
            self.logger.addHandler(handler)

        def log_info(self, message):  
            self.logger.info(message)

        def log_error(self, message, e=None):  
            if e:  
                self.logger.error(f"{message}: {e}")  
            else:  
                self.logger.error(message)

    # In another file, e.g., data_loader.py
    def load_data(path, debug_manager):  
        """Loads data from a specified path."""  
        try:  
            with open(path, 'r') as f:  
                return f.read()  
        except FileNotFoundError as e:  
            debug_manager.log_error(f"File not found at '{path}'", e)  
            return None

    if __name__ == '__main__':  
        # Example usage in main.py  
        dbg = DebugManager(output_mode='terminal')

        data = load_data('non_existent_file.txt', dbg)  
        if data is None:  
            dbg.log_info("Failed to load data, proceeding gracefully.")
    ```

### **3.7 Code File Splitting**

* **Principle:** When the overall code is too large, complex, or classes have high extensibility, it should be split into different files. For example, in a deep learning project, it should be split into dataset.py, models.py, runner.py, config.py, and main.py, etc.  
* **Example Structure:**  
    ```
    my_project/  
    ├── dataset.py  
    ├── models.py  
    ├── runner.py  
    ├── config.py  
    └── main.py
    ```

    * dataset.py: Handles data loading, transformation, and preprocessing.  
    * models.py: Defines all model architectures, such as neural networks.  
    * runner.py: Contains training, validation, and testing execution logic.  
    * config.py: Stores all configurable parameters.  
    * main.py: The entry point of the program, handling argument parsing and calling functions from other modules to execute the main pipeline.