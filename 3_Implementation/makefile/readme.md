# Name of the project
PROJECT_NAME = ATM banking

# Output directory
BUILD = build

# All source code files
SRC = main.c\
src/balance.c\
src/deposit.c\
src/error.c\
src/exit.c\
src/Menu.c\
src/withdraw.c\

# All test source files
TEST_SRC = src/balance.c\
src/deposit.c\
src/error.c\
src/exit.c\
src/Menu.c\
src/withdraw.c\
test/test.c\
unity/unity.c\

# To Check OS is Windows or Linux

ifdef OS
	RM = del /q
	FixPath = $(subst /,\,$1)
	EXEC = exe
else
	ifeq ($(shell uname), Linux)
		RM = rm -rf
		FixPath = $1
		EXEC = out
	endif
endif

TEST_OUTPUT = $(BUILD)/Test_$(PROJECT_NAME).out

# All include folders with header files
INC	= -Iinc\
-Iunity\

#Library Inlcudes
#if working with CUnit 
#INCLUDE_LIBS = -lcunit
INCLUDE_LIBS =

# Project Output name
PROJECT_OUTPUT = $(BUILD)/$(PROJECT_NAME).out

# Document files
DOCUMENTATION_OUTPUT = documentation/html

# Default target built
$(PROJECT_NAME):all

# Run the target even if the matching name exists
.PHONY: run clean test doc all

all: $(SRC) $(BUILD)
	gcc $(SRC) $(INC) -o $(PROJECT_OUTPUT).out

# Call `make run` to run the application
run:$(PROJECT_NAME)
	./$(PROJECT_OUTPUT).out

# Document the code using Doxygen
#doc:
#	make -C ./documentation

# Build and run the unit tests
test:$(BUILD)
	gcc $(TEST_SRC) $(INC) -o $(TEST_OUTPUT)
	./$(TEST_OUTPUT)
#Coverage
coverage:$(BUILD)
	g++ -fprofile-arcs -ftest-coverage -fPIC -O0 $(TEST_SRC) $(INC) -o $(TEST_OUTPUT)
	./$(TEST_OUTPUT)

# Remove all the built files, invoke by `make clean`
valgrind:$(BUILD)
	gcc $(TEST_VALGRIND) $(INC) -o build/Test_ATM.out

clean:
	$(RM) $(call FixPath,$(BUILD)/*)

# Create new build folder if not present
$(BUILD):
	mkdir build
