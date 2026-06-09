# 交叉编译器
CROSS_COMPILE = aarch64-rockchip1031-linux-gnu-
CC  = $(CROSS_COMPILE)gcc
CXX = $(CROSS_COMPILE)g++

# SDL2 安装路径
SDL_INSTALL_DIR = /home/alientek/czh/SDL-release-2.30.8/install

TARGET = InfoNES

# 源文件（mapper/*.cpp 由 InfoNES_Mapper.cpp 内部 #include，不需单独编译）
CORE_DIR  = ..

CORE_SRCS = $(CORE_DIR)/K6502.cpp \
            $(CORE_DIR)/InfoNES.cpp \
            $(CORE_DIR)/InfoNES_Mapper.cpp \
            $(CORE_DIR)/InfoNES_pAPU.cpp

PLATFORM_SRCS = InfoNES_System_RK.cpp

SRCS = $(CORE_SRCS) $(PLATFORM_SRCS)
OBJS = $(SRCS:.cpp=.o)

# 编译选项
INCLUDES = -I$(CORE_DIR) -I$(SDL_INSTALL_DIR)/include -I$(SDL_INSTALL_DIR)/include/SDL2
CXXFLAGS = $(INCLUDES) -O3 -pipe
CXXFLAGS_NOOPT = $(INCLUDES) -O0 -pipe
LDFLAGS  = -L$(SDL_INSTALL_DIR)/lib -lSDL2 -lm -lpthread -lstdc++

.PHONY: all clean

all: $(TARGET)

$(TARGET): $(OBJS)
	$(CXX) -o $@ $(OBJS) $(LDFLAGS)

# InfoNES_pAPU.cpp 用 -O0 避免 gcc 优化 bug
$(CORE_DIR)/InfoNES_pAPU.o: $(CORE_DIR)/InfoNES_pAPU.cpp
	$(CXX) $(CXXFLAGS_NOOPT) -c $< -o $@

# 通用编译规则
%.o: %.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $@

clean:
	rm -f $(OBJS) $(TARGET)

# End of Makefile
