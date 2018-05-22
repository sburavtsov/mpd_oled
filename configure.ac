
AC_PREREQ([2.69])
AC_INIT([mpd_oled], [0.2], [adrian@antiprism.com])

AM_INIT_AUTOMAKE([1.9 foreign])

AC_CONFIG_SRCDIR([src/glcdfont.c])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])

# Checks for programs.
AC_PROG_CXX
AC_PROG_CC

# Set compiler
AC_LANG([C++])
#AX_CXX_COMPILE_STDCXX(11)

# Add -fno-strict-aliasing to CFLAGS if accepted
if $CXX -fno-strict-aliasing -S -o /dev/null -xc /dev/null >/dev/null 2>&1;
then
   CPPFLAGS="-fno-strict-aliasing $CFLAGS"
fi

# Checks for header files.
AC_CHECK_HEADERS([fcntl.h locale.h stdint.h stdlib.h string.h sys/ioctl.h sys/socket.h sys/time.h unistd.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_CHECK_HEADER_STDBOOL
AC_C_INLINE
AC_TYPE_INT16_T
AC_TYPE_INT8_T
AC_TYPE_OFF_T
AC_TYPE_SIZE_T
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT64_T
AC_TYPE_UINT8_T

# Checks for library functions.
AC_FUNC_MALLOC
AC_FUNC_MMAP
AC_CHECK_FUNCS([gettimeofday memset mkfifo munmap select setlocale socket strerror strspn strstr strtol])

#AC_CHECK_FILE([/volumio],[PLAYER=VOLUMIO], [
AC_CHECK_FILE([/volumio],[PLAYERDETECT=VOLUMIO], [
  AC_CHECK_FILE([/srv/http/command/rune_shutdown], [PLAYERDETECT=RUNEAUDIO], [
    AC_CHECK_FILE([/var/www/command/moode.php], [PLAYERDETECT=MOODE], [
      PLAYERDETECT=MPD
    ])
  ])
])

echo "Detected player: $PLAYERDETECT"
if test x"${PLAYER}" = x""; then
   PLAYER=$PLAYERDETECT
   echo "Building for detected player: $PLAYERDETECT"
else
   PLAYER=${PLAYER}
   echo "Building for player set with PLAYER: $PLAYER"
fi
  

AC_SUBST(PLAYER)

inst_msg="Install a development version of this library and rerun ./configure"
AC_LANG_PUSH([C++])
# Checks for libraries.
AC_CHECK_LIB([mpdclient], [mpd_connection_new], [], [AC_MSG_ERROR([$inst_msg])])
AC_CHECK_HEADER([mpd/client.h], [], [AC_MSG_ERROR([$inst_msg])])
AC_CHECK_LIB([pthread], [pthread_create], [], [AC_MSG_ERROR([$inst_msg])])
AC_CHECK_HEADER([pthread.h], [], [AC_MSG_ERROR([$inst_msg])])

PLAYERLIBS="-lmpdclient -lpthread"
if test x"$PLAYER" = x"VOLUMIO" ; then
  AC_CHECK_LIB([curl], [curl_easy_init], [], [AC_MSG_ERROR([$inst_msg])])
  AC_CHECK_HEADER([curl/curl.h], [], [AC_MSG_ERROR([$inst_msg])])
  PLAYERLIBS="$PLAYERLIBS -lcurl"
  AC_CHECK_LIB([jsoncpp], [main], [], [AC_MSG_ERROR([$inst_msg])])
  AC_CHECK_HEADER([jsoncpp/json/json.h], [], [AC_MSG_ERROR([$inst_msg])])
  PLAYERLIBS="$PLAYERLIBS -ljsoncpp"
else
  if test x"$PLAYER" = x"RUNEAUDIO" ; then
    AC_CHECK_LIB([i2c], [i2c_smbus_write_byte_data], [], [AC_MSG_ERROR([$inst_msg])])
    PLAYERLIBS="$PLAYERLIBS -li2c"
  else
    if test x"$PLAYER" = x"MOODE" ; then
      PLAYERLIBS="$PLAYERLIBS"
    fi
  fi
fi

AC_SUBST(PLAYERLIBS)

AC_LANG_POP([C++])


AC_CONFIG_FILES([Makefile
                 src/Makefile
                 ])

AC_OUTPUT
