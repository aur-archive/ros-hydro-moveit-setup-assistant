
pkgdesc="ROS - Generates a configuration package that makes it easy to use MoveIt!."
url='http://www.ros.org/'

pkgname='ros-hydro-moveit-setup-assistant'
pkgver='0.5.8'
_pkgver_patch=0
arch=('i686' 'x86_64')
pkgrel=1
license=('BSD')

ros_makedepends=(ros-hydro-moveit-ros-planning
  ros-hydro-catkin
  ros-hydro-moveit-ros-visualization
  ros-hydro-moveit-core)
makedepends=('cmake' 'git' 'ros-build-tools'
  ${ros_makedepends[@]}
  yaml-cpp)

ros_depends=(ros-hydro-moveit-ros-planning
  ros-hydro-xacro
  ros-hydro-moveit-ros-visualization
  ros-hydro-moveit-core)
depends=(${ros_depends[@]}
  yaml-cpp)

_tag=release/hydro/moveit_setup_assistant/${pkgver}-${_pkgver_patch}
_dir=moveit_setup_assistant
source=("${_dir}"::"git+https://github.com/ros-gbp/moveit_setup_assistant-release.git"#tag=${_tag}
        'yamlcpp.patch')
md5sums=('SKIP'
         'e8e56ba7eb5e661608b85c1a79ddf9c1')

build() {
  # Use ROS environment variables
  /usr/share/ros-build-tools/clear-ros-env.sh
  [ -f /opt/ros/hydro/setup.bash ] && source /opt/ros/hydro/setup.bash

  # Apply patch
  msg "Patching source code"
  cd ${srcdir}/${_dir}
  git apply ${srcdir}/yamlcpp.patch

  # Create build directory
  [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
  cd ${srcdir}/build

  # Fix Python2/Python3 conflicts
  /usr/share/ros-build-tools/fix-python-scripts.sh ${srcdir}/${_dir}

  # Build project
  cmake ${srcdir}/${_dir} \
        -DCMAKE_BUILD_TYPE=Release \
        -DCATKIN_BUILD_BINARY_PACKAGE=ON \
        -DCMAKE_INSTALL_PREFIX=/opt/ros/hydro \
        -DPYTHON_EXECUTABLE=/usr/bin/python2 \
        -DPYTHON_INCLUDE_DIR=/usr/include/python2.7 \
        -DPYTHON_LIBRARY=/usr/lib/libpython2.7.so \
        -DSETUPTOOLS_DEB_LAYOUT=OFF
  make
}

package() {
  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}/" install
}
