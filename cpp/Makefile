# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    Makefile                                           :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: fmauguin <fmauguin@student.42.fr >         +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2022/07/25 13:22:33 by gtoubol           #+#    #+#              #
#    Updated: 2022/12/05 14:10:53 by fmauguin         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

SHELL=/bin/bash

# List of external assets
# -------------------------------------------------------------------------
DISPLAYURL =	https://cdn.intra.42.fr/document/document/22648/display_linux.tar.gz
DISPLAY_OUT =	matrix_display

# List of all the sources (.cpp)
# -------------------------------------------------------------------------
SRCS =		main.cpp													\


# List of the related directories
# -------------------------------------------------------------------------
OBJS_DIR =	objs
SRCS_DIR =	src
HEAD_DIR =	include
TPP_DIR =	templates

# List of all compilation options
# -------------------------------------------------------------------------
CXX = 		c++
CXXFLAGS =	-Wall -Wextra -Werror
DEPSFLAGS =	-MMD -MP

# Description of the final target
# -------------------------------------------------------------------------
NAME =		project

# Libraries
# -------------------------------------------------------------------------

# General rules on makefile
# -------------------------------------------------------------------------
OBJS = 		$(addprefix $(OBJS_DIR)/,$(SRCS:.cpp=.o))
DEPS =		$(addprefix $(OBJS_DIR)/,$(SRCS:.cpp=.d))

INCLUDES =	$(addprefix -I,$(HEAD_DIR))

RM =		rm -f
RMDIR =		rm -rf

vpath %.c $(SRCS_DIR)
vpath %.h $(HEAD_DIR)

# Color
# -------------------------------------------------------------------------
_GREY		= \033[30m
_RED		= \033[31m
_GREEN		= \033[32m
_YELLOW		= \033[33m
_BLUE		= \033[34m
_PURPLE		= \033[35m
_CYAN		= \033[36m
_WHITE		= \033[37m
_NO_COLOR	= \033[0m

all:		$(NAME)

$(NAME):	$(OBJS)
			$(CXX) $(CXXFLAGS) $(INCLUDES) -o $@ $(OBJS)
			@echo -e "$(_GREEN)$(NAME): Compilation done$(_NO_COLOR)"

$(OBJS_DIR)/%.o: $(SRCS_DIR)/%.cpp
	@if [ ! -d $(dir $@) ]; then \
		mkdir -p $(dir $@); \
		echo -e "\n$(_BLUE)$(dir $@): Create$(_NO_COLOR)"; \
	fi
	$(CXX) $(CXXFLAGS) ${DEPSFLAGS} $(INCLUDES) -o $@ -c $<

clean:		dclean
			@$(RM) $(OBJS)
			@if [ -d $(OBJS_DIR) ]; then \
				find $(OBJS_DIR) -type d | xargs rmdir -p --ignore-fail-on-non-empty; \
				echo -e "$(_RED)$(OBJS_DIR): Removed$(_NO_COLOR)"; \
			fi

fclean:		clean oclean
			@$(RM) $(NAME)
			@echo -e "$(_RED)$(NAME): Removed$(_NO_COLOR)"

re:			fclean all

oclean:
			@if [ -d $(DISPLAY_OUT) ]; then \
				$(RMDIR) $(DISPLAY_OUT); \
				echo -e "\n$(_RED)$(DISPLAY_OUT): Removed$(_NO_COLOR)"; \
			fi

install :	oclean
			@echo -e "\n$(_BLUE)Downloading the display_linux.tar.gz file...$(_NO_COLOR)"
			@curl -O $(DISPLAYURL)
			@echo -e "\n$(_BLUE)Extracting the display_linux.tar.gz file...$(_NO_COLOR)"
			@tar -xvf display_linux.tar.gz
			@echo -e "\n$(_BLUE)Removing the display_linux.tar.gz file...$(_NO_COLOR)"
			@rm -f display_linux.tar.gz
			@echo -e "$(_GREEN)Install done.$(_NO_COLOR)"

.PHONY:		all clean fclean re install oclean
.NOTPARALLEL: re

# Library rules
# -----------------------------------------------------------------------

# General dependences management
# ------------------------------------------------------------------------
# $(OBJS_DIR)/%.d: $(SRCS_DIR)/%.cpp
# 	@if [ ! -d $(dir $@) ]; then \
# 		mkdir -p $(dir $@); \
# 		echo -e "\n$(_BLUE)$(dir $@): Create$(_NO_COLOR)"; \
# 	fi
# 	$(CXX) -MM -MT $(@:.d=.o) $(CXXFLAGS) $(INCLUDES) $< > $@

dclean:
			@$(RM) $(DEPS)
			echo -e "$(_RED)$(DEPS): Removed$(_NO_COLOR)"

.PHONY:		dclean
.SILENT:    $(DEPS) dclean

-include	$(DEPS)