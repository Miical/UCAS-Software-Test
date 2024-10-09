# Makefile for coverage analysis using gcov and lcov

# Variables
CC = gcc
CFLAGS = -fprofile-arcs -ftest-coverage -g
SRC = game.c
OBJ = $(SRC:.c=.o)
EXEC = game
OUTPUT_DIR = coverage_html

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
	rm -rf $(OUTPUT_DIR)

# Phony targets
.PHONY: all run coverage clean

