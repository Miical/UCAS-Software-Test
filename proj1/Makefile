# Makefile for coverage analysis using gcov and lcov

# Variables
CC = gcc
CFLAGS = -fprofile-arcs -ftest-coverage -g
SRC = game.c
OBJ = $(SRC:.c=.o)
EXEC = game
OUTPUT_DIR = coverage_html
TESTCASE_DIR = white-box-testcases
RESULT_LOG = results.log

# Targets
all: $(EXEC) coverage

# Compile the source code
$(EXEC): $(OBJ)
	$(CC) -o $@ $^ -lgcov

# Compile with coverage flags
%.o: %.c
	$(CC) $(CFLAGS) -c $<

# Run the program
run: $(EXEC)
	./$(EXEC)

# Run all test cases in the testcases directory
run_all: $(EXEC)
	@echo "Running all test cases in $(TESTCASE_DIR)..."
	@echo "" > $(RESULT_LOG)  # Clear result log
	@for testcase in $(TESTCASE_DIR)/*.txt; do \
		echo "Running ./game < $$testcase" | tee -a $(RESULT_LOG); \
		./$(EXEC) < $$testcase | tee -a $(RESULT_LOG); \
	done
	@echo "All test cases executed. Results saved in $(RESULT_LOG)."
	@echo "Generating coverage report..."
	gcov $(SRC)
	@echo "Generating HTML report..."
	lcov --directory . --capture --output-file coverage.info
	genhtml coverage.info --output-directory $(OUTPUT_DIR)

# Generate coverage report
coverage: run
	@echo "Generating coverage report..."
	gcov $(SRC)
	@echo "Generating HTML report..."
	lcov --directory . --capture --output-file coverage.info
	genhtml coverage.info --output-directory $(OUTPUT_DIR)

# Clean up generated files
clean:
	rm -f $(OBJ) $(EXEC) *.gcda *.gcno coverage.info *.gcov
	rm -rf $(OUTPUT_DIR) $(RESULT_LOG)

# Phony targets
.PHONY: all run coverage clean

