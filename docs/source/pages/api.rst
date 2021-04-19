======================
Moblabpy Documentation
======================

.. py:class:: PROPS

    A class that contains all the configurations of the Moblabpy module

    .. py:attribute:: FPS
        :type: int
        :value: 15
        
        The frame per second of the generated video.

    .. py:attribute:: BPS
        :type: int
        :value: 16
        
        The number of color matrix in each frame. It can be set by :py:meth:`set_BPS()` .

    .. py:attribute:: ROW
        :type: int
        :value: 4
        
        The number of row of color matrix in each frame. :py:attr:`ROW` must be equal 
        to :py:attr:`COL` and they are defined by the square root of :py:attr:`BPS` .

    .. py:attribute:: COL
        :type: int
        :value: 4
        
        Same as :py:attr:`ROW` .

    .. py:attribute:: INFO_BIT_SIZE
        :type: int
        :value: 50
        
        The height and width of each color matrix. It is defined by :py:attr:`ROW` divides 200.

    .. py:attribute:: START_BIT
        :type: str
        :value: "0"
        
        The string representation of start bit and end bit of the video.

    .. py:attribute:: END_BIT
        :type: str
        :value: "0"
        
        Same as :py:attr:`START_BIT` .

    .. py:attribute:: VID_SOURCE
        :type: str
        :value: "./16bps.mp4"
        
        The video name of the generated video. It will be set by :py:meth:`set_BPS()` automatically if 
        py:attr:`BPS` is modified.
    
    .. py:attribute:: SCALE
        :type: int
        :value: 2
        
        The scale of the window size of the video player embedded in the :py:class:`MobLabPy` .
    
    .. py:method:: set_BPS(bps)

        Set the :py:attr:`BPS` of the video. It will also change the value of :py:attr:`ROW`, 
        :py:attr:`COL`, :py:attr:`INFO_BIT_SIZE` and :py:attr:`VID_SOURCE` automatically.

        :param bps: The BPS of the video
        :type bps: int

.. py:class:: MobLabPy

    A class that allows users to connect their ip camera with the program to simulate a
    telecommunication system. It use the python :py:mod:`multiprocessing` modules to 
    reduce the time delay of decoding images and capturing images. The program uses 
    :py:obj:`multiprocessing.Pipe` object to send images between the :py:func:`send_frame()` 
    and :py:func:`recv_frame()` .

    .. py:function:: __init__(self, ip_address, bit_mess, props = PROPS)

        Initialize the member variable of the :py:class:`MobLabPy` object.

        :param ip_address: The ip address of the IP camera
        :type ip_address: str
        :param bit_mess: The transmitted bit sequence
        :type bit_mess: str
        :param props: A :py:class:`PROPS` class

    .. py:function:: start(self)

        Start getting frames from the ip camera and send it to the decoding function.

    .. py:function:: get_bit_seq(self)

        Return the bit sequence of the :py:class`MobLabPy` object.

        :returns bit_seq: The received bit sequence
        :rtype: list(char)

.. py:method:: img_to_bit_seq(frame, pt1 = (), pt2 = (), props = PROPS)

    Convert black and white color to "0" and "1" respectively.

    :param frame: Image.
    :type frame: :py:class:`numpy.ndarray`
    :param pt1: Vertex of the area bounded by qr codes, defaults to ()
    :type pt1: tuple(int, int)
    :param pt2: Vertex of the area bounded by qr codes opposite to pt1, defaults to ()
    :type pt2: tuple(int, int)
    :param props: The :py:class:`PROPS` object that contains the configurations of the program
    :type props: :class:`PROPS`
    :returns bit_seq, pt1, pt2: Decoded bit sequence and vertices of the area bounded by qr codes in opposite direction.
    :rtype: tuple(str, tuple(int, int), tuple(int, int))
    :raises IndexError: If it fails to calculate the mean of the RGB of the color matrix

.. py:method:: check_orientation(frame, pt1, pt2)

    Check the orientation of the image according to the qr codes in the corner.

    :param frame: Image
    :type frame: :py:class:`numpy.ndarray`
    :param pt1: Vertex of the area bounded by qr codes.
    :type pt1: tuple(int, int)
    :param pt2: Vertex of the area bounded by qr codes opposite to pt1.
    :type pt2: tuple(int, int)
    :return result: Number of rotation needed to be performed
    :rtype: int
    :raises IndexError: If it fails to locate the qr codes      

.. py:method:: point_rotation(pt1, pt2, width, rotation)

    Perform the rotation of vertices of the color matrix area.

    :param pt1: Vertex of the color matrix
    :type pt1: tuple(int, int)
    :param pt2: Vertex of the color matrix opposite to pt1
    :type pt2: tuple(int, int)
    :param width: Width of the image that contains the color matrix
    :type: int
    :param rotation: Number of rotation that needed to be performed
    :type rotation: int
    :returns pt1, pt2: Vertices of the color matrix area after rotation
    :type: tuple(tuple(int, int), tuple(int, int))

.. py:method:: find_corner(frame, pt1 = (), pt2 = ())

    Find the vertex of the color matrix surrounded by but exclude qr codes.

    :param frame: Image
    :type frame: :py:class:`numpy.ndarray`
    :param pt1: Vertex of the area include qr codes, defaults to ()
    :type pt1: tuple(int, int)
    :prarm pt2: Vertex of the area include qr codes opposite to pt1, defaults to ()
    :type pt2: tuple(int, int)
    :returns size, pt1, pt2: size of the color matrix area and its vertices in opposite direction
    :rtype: tuple(int, tuple(int, int), tuple(int, int))

.. py:method:: find_min_and_max(points, min_pt, max_pt)

    Find the minimum and maximum xy-points in a list and the provides points.

    :param points: A list of points
    :type points: list(int)
    :param min_pt: The minimum points provided
    :type min_pt: list(int)
    :param max_pt: The maximum points provided
    :type max_pt: list(int)
    :returns min_pt, max_pt: The minimum and maximum xy-points
    :rtype: tuple(intuple(int, int)t, tuple(int, int))

.. py:method:: all_zeros_or_ones(bit_seq)

    Check if the bit sequence contains only 0 or 1.

    :param bit_seq: The bit sequence needed to be checked
    :type bit_seq: str
    :return: "True" if the bit sequence only contains 0 or 1, "False" otherwise
    :rtype: bool
    :raises ValueError: it input contains non numerical values

.. py:method:: str_to_ascii_seq(my_str = "Apple")

    Convert each character in a string to its corresponding ascii code.

    :param my_str: The string wanted to be converted, defaults to `Apple`
    :type my_str: str
    :return ascii_arr: A list of integer that contains all the ascii code of each character in my_str
    :rtype: list(int)

.. py:method:: ascii_seq_to_bit_seq(ascii_arr)

    Convert a list of ascii code to its binary representation.

    :param ascii_arr: A list that contains ascii code
    :type ascii_arr: list(int)
    :return bit_arr: An integer binary list
    :rtype: list(int)

.. py:method:: bit_seq_to_ascii_seq(bit_arr)

    Perform byte conversion of an array.

    :param bit_arr: An integer binary array
    :type bit_arr: list(int)
    :return ascii_arr: A list contains all the corresponding ascii code
    :rtype: list(int)        

.. py:method:: ascii_seq_to_str(ascii_arr)

    Convert a list of ascii code to its corresponding character.

    :param ascii_arr: A list that contains ascii code
    :type ascii_arr: list(int)
    :return my_str: The character format of the ascii list
    :rtype: str

.. py:method:: append_zeros(bit_seq, props = PROPS, is_str = False)

    Append zeros until the length of the input equals to the products of desired length.

    :param bit_seq: Integer binary array
    :type bit_seq: list(int)
    :param props: :py:class:`PROPS` object that contains the configurations of the program.
    :type props: :py:class:`PROPS`
    :return bit_arr: An integer binary array which length is the product of the second parameter
    :rtype: list(int)

.. py:method:: generate_img(dir_name, my_str = "Hello World", if_bin = False, props = PROPS)

    Generate images that contain color matrix of my_str parameter, with black represents 0 and white represents 1, and
    paste them with the qr codes (finder patterns) together. The generated images will be saved at the dir_name 
    directory.

    :param dir_name: The directory name used to save the image generated
    :type dir_name: str
    :param my_str: The integer binary array or string which will be convert to color code formats, defaults to "Hello World"
    :type my_str: str, list(int)
    :param if_bin: `True` if my_str is an integer binary array, `False` otherwise, defaults to `False`
    :type if_bin: bool
    :param props: the :py:class:`PROPS` object that contains the properties of the program
    :type props: :py:class:`PROPS`   

.. py:method:: generate_res()

    Generate the res folder which contains all the head and end messages, start and end signals and finder patterns.

.. py:method:: generate_video(video_name = "16bps.mp4", mystr = "Hello World", if_bin = False, props = PROPS)

    Generate a video which contains start and end messages, start and end signals, finder patterns and color matrix
    of the mystr parameter, with desired fps.

    :param video_name: The name of the generated video in mp4 format
    :type video_name: str
    :param mystr: The message that will be converted to color code, defaults to "Hello World"
    :type mystr: str, list(int)
    :param if_bin: `True` if mystr is an integer binary array, `False` otherwise, defaults to `False`
    :type if_bin: bool
    :param props: the :py:class:`PROPS` object that contains the properties of the program
    :type props: :py:class:`PROPS`

.. py:method:: recv_frame(fps, is_pilot_bit_found , recver, bit_seq, flag, bit_mess, props = PROPS)

    Funtion that received frames from the sender function. It does all the manipulation of the captured iamge
    here, such as detecting start and end signals, checking rotations and decoding color codes back to 0 and 1.

    :param fps: A :py:obj:`multiprocessing.Value` object which represents the fps of the camera
    :type fps: :py:obj:`multiprocessing.Value`
    :param is_pilot_bit_found: A :py:obj:`multiprocessing.Value` object which is `True` if the start signal is detected, `False` otherwise
    :type is_pilot_bit_found: :py:obj:`multiprocessing.Value`
    :param recver: A :py:obj:`multiprocessing.Pipe` object that received images sent from the sender function
    :type recver: :py:obj:`multiprocessing.Pipe`
    :param bit_seq: A :py:obj:`multiprocessing.Value` object whcih is the decoded bit sequence of received images 
    :type bit_seq: :py:obj:`multiprocessing.Value`
    :param flag: A :py:obj:`multiprocessing.Value` object which is the state of the play video button
    :type flag: :py:obj:`multiprocessing.Value`
    :param bit_mess: The transmitted bit sequence
    :type bit_mess: str
    :param props: The :py:class:`PROPS` object that contains the properties of the program
    :type props: :py:class:`PROPS`

.. py:method:: vid_player(vid_source, flag, props = PROPS)

    A video player will be appeared if the play video button is clicked

    :param vid_source: The name of the video to pe played
    :type vid_source: str
    :param flag: A :py:obj:`multiprocessing.Value` object which is the state of the play video button
    :type flag: :py:obj:`multiprocessing.Value`
    :param props: The :py:class:`PROPS` object that contains the properties of the program
    :type props: :py:class:`PROPS`

.. py:method:: send_frame(fps, is_pilot_bit_found, sender, ip_address, props = PROPS)

    A sender function which gets images from the connected camera and send it to the receiver function.

    :param fps: A :py:obj:`multiprocessing.Value` object which represents the fps of the camera
    :type fps: :py:obj:`multiprocessing.Value`
    :param is_pilot_bit_found: A :py:obj:`multiprocessing.Value` object which is `True` if the start signal is detected, `False` otherwise
    :type is_pilot_bit_found: :py:obj:`multiprocessing.Value`
    :param sender: A :py:obj:`multiprocessing.Pipe` object that send images captured by the connected camera
    :type sender: :py:obj:`multiprocessing.Pipe`
    :param ip_address: IP address of the camera that is going to be connected
    :type ip_address: str
    :param props: The :py:class:PROPS object that contains the properties of the program
    :type props: :py:class:`PROPS`

.. py:method:: encode(D, G)

    Perform matrix multiplication on parameter D and G, which is D x G.

    :param D: A 1 x k matrix
    :type D: list(int)
    :param G: A generator matrix
    :type G: :py:class:`numpy.ndarray`
    :return C: A 1 x n matrix
    :rtype: :py:class:`numpy.ndarray`
    :raises ValueError: If the number of columns in the first matrix not equals to the number of rows in the second matrix
    :raises UFuncTypeError: If both parameters are not integer :py:class:`numpy.ndarray`

.. py:method:: syndrome(R, H)

    Calculate the syndrome of the received bit sequence.

    :param R: A 1 x n matrix
    :type R: list(int)
    :param H: A parity check matrix
    :type H: :py:class:`numpy.ndarray`

.. py:class:: monitor

    .. py:function:: __init__(self, master, bit_mess)

        Defines the monitor window.

        :param master: Root window
        :type master: Tk
        :param bit_mess: The bit sequence to be sent
        :type bit_mess: str

    .. py:function:: update_recv(self, bit_seq)

        Update the received and error bits textbox upon call.

        :param bit_seq: The bits that are collected from the receiver
        :type master: str

    .. py:function:: button_toggle(self)

        Switch the status of the button to True, which indicates it has been pressed.

    .. py:function:: get_button_status(self)

        Return the status of the button.
        
        :return: The status of the button, whether it has been pressed
        :rtype: bool

    .. py:function:: close_windows(self)

        Destroy the root window.
    
.. py:class:: VidCap

    .. py:function:: __init__(self, scale, vidsource=0)

        Defines the video to be played and its porperty.

        :param scale: The scale of which the video is being played. The higher the number, the bigger the video
        :type scale: int
        :param vidsource: The file path of the video
        :type vidsource: str

    .. py:function:: get_fps(self)

        Return the fps of the video.

        :return: The fps of the video
        :rtype: int 

    .. py:function:: get_height(self)

        Return the height of the video.

        :return: The height of the video
        :rtype: int

    .. py:function:: get_width(self)

        Return the width of the video.

        :return: The width of the video
        :rtype: int 

    .. py:function:: get_frame(self)

        Get one frame from the video.

        :returns ret, frame: whether a frame is successfully grabbed; the image from the video, None otherwise.
        :rtype: tuple(:py:class:`bool`, :py:class:`numpy.ndarray`)   

    .. py:function:: reset(self, vidsource=0)

        Reset the video source and start from the beginning.

    .. py:function:: __del__(self)

        Release the video when the window is closed.

.. py:class:: player

    .. py:function:: __init__(self, master, scale, vidsource=0)

        Defines the video player window.

        :param master: Root window
        :type master: Tk
        :param scale: The scale of which the video is being played. The higher the number, the bigger the video
        :type scale: int
        :param vidsource: The file path of the video
        :type vidsource: str

    .. py:function:: update(self)

        Update the video canvas to display the next frame.

    .. py:function:: restart_vid(self)

        Replay the video. 

    .. py:function:: close_windows(self)

        Destroy the root window.