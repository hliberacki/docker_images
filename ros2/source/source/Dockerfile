# This is an auto generated Dockerfile for ros2:source
# generated from docker_images_ros2/source/create_ros_image.Dockerfile.em

ARG FROM_IMAGE=osrf/ros2:devel
FROM $FROM_IMAGE

# install packages
RUN apt-get update && apt-get install -q -y --no-install-recommends \
    libasio-dev \
    libssl-dev \
    libtinyxml2-dev \
    wget \
    && rm -rf /var/lib/apt/lists/*
ARG ROS_DISTRO=galactic
ENV ROS_DISTRO=$ROS_DISTRO
ENV ROS_VERSION=2 \
    ROS_PYTHON_VERSION=3

WORKDIR $ROS2_WS

RUN wget https://raw.githubusercontent.com/ros2/ros2/$ROS_DISTRO-release/ros2.repos \
    && vcs import src < ros2.repos

# install dependencies
RUN apt-get update && rosdep install -y \
    --from-paths src \
    --ignore-src \
    --skip-keys " \
        console_bridge \
        fastcdr \
        fastrtps \
        libopensplice69 \
        rti-connext-dds-6.0.1 \
        urdfdom_headers" \
    && rm -rf /var/lib/apt/lists/*

# build source
RUN colcon \
    build \
    --symlink-install \
    --mixin build-testing-on release \
    --cmake-args --no-warn-unused-cli

ARG RUN_TESTS
ARG FAIL_ON_TEST_FAILURE
RUN if [ ! -z "$RUN_TESTS" ]; then \
        colcon test; \
        if [ ! -z "$FAIL_ON_TEST_FAILURE" ]; then \
            colcon test-result; \
        else \
            colcon test-result || true; \
        fi \
    fi
