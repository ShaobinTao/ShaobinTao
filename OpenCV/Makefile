CXX = g++

CXXFLAGS = -Wall -std=c++0x -I/usr/local/include

LDFLAGS = -L/usr/local/lib -L/usr/lib

LDLIBS = -lopencv_core -lopencv_highgui -lopencv_imgcodecs -lopencv_imgproc -lopencv_video -lopencv_calib3d -lopencv_dnn -lopencv_features2d -lopencv_flann -lopencv_gapi -lopencv_photo -lopencv_stitching

TARGET = hello

SOURCE = $(TARGET).cpp

TARGET:
	$(CXX) -o $(TARGET) $(CXXFLAGS) $(LDFLAGS) $(SOURCE) $(LDLIBS)

.PHONY: clean
clean:
	rm -f *.o *~ $(TARGET)
