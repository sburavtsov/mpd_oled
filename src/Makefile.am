bin_PROGRAMS = mpd_oled

mpd_oled_SOURCES = bcm2835.c glcdfont.c \
		   \
		   Adafruit_GFX.cpp ArduiPi_OLED.cpp display.cpp main.cpp \
		   programopts.cpp status.cpp status_msg.cpp timer.cpp \
		   ultragetopt.cpp utils.cpp  \
		   \
		   Adafruit_GFX.h ArduiPi_OLED.h ArduiPi_OLED_lib.h bcm2835.h \
		   display.h display_info.h gfxfont.h iconv_wrap.h \
		   programopts.h  status.h status_msg.h timer.h \
		   ultragetopt.h utils.h

#mpd_oled_LDADD = $(PLAYERLIBS)
mpd_oled_CPPFLAGS = -D$(PLAYER)


