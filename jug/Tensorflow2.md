# TensorFlow 2

---

[toc]



## Overview





## install

```shell
(grpc01) [chenchen@localhost bin]$ ./python3 -m pip install tensorflow
Collecting tensorflow
  Downloading tensorflow-2.8.0-cp37-cp37m-manylinux2010_x86_64.whl (497.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 497.5/497.5 MB 4.0 MB/s eta 0:00:00
Collecting flatbuffers>=1.12
  Downloading flatbuffers-2.0-py2.py3-none-any.whl (26 kB)
Collecting tensorflow-io-gcs-filesystem>=0.23.1
  Downloading tensorflow_io_gcs_filesystem-0.24.0-cp37-cp37m-manylinux_2_12_x86_64.manylinux2010_x86_64.whl (2.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.1/2.1 MB 10.7 MB/s eta 0:00:00
Collecting numpy>=1.20
  Downloading numpy-1.21.5-cp37-cp37m-manylinux_2_12_x86_64.manylinux2010_x86_64.whl (15.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 15.7/15.7 MB 9.8 MB/s eta 0:00:00
Collecting wrapt>=1.11.0
  Downloading wrapt-1.14.0-cp37-cp37m-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl (75 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 75.2/75.2 KB 20.9 MB/s eta 0:00:00
Requirement already satisfied: protobuf>=3.9.2 in /home/chenchen/tmp/venv/grpc01/lib/python3.7/site-packages (from tensorflow) (3.19.4)
Collecting absl-py>=0.4.0
  Downloading absl_py-1.0.0-py3-none-any.whl (126 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 126.7/126.7 KB 35.0 MB/s eta 0:00:00
Collecting termcolor>=1.1.0
  Downloading termcolor-1.1.0.tar.gz (3.9 kB)
  Preparing metadata (setup.py) ... done
Collecting opt-einsum>=2.3.2
  Downloading opt_einsum-3.3.0-py3-none-any.whl (65 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 65.5/65.5 KB 18.9 MB/s eta 0:00:00
Collecting typing-extensions>=3.6.6
  Downloading typing_extensions-4.1.1-py3-none-any.whl (26 kB)
Collecting libclang>=9.0.1
  Downloading libclang-13.0.0-py2.py3-none-manylinux1_x86_64.whl (14.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 14.5/14.5 MB 9.0 MB/s eta 0:00:00
Collecting google-pasta>=0.1.1
  Downloading google_pasta-0.2.0-py3-none-any.whl (57 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 57.5/57.5 KB 11.8 MB/s eta 0:00:00
Collecting gast>=0.2.1
  Downloading gast-0.5.3-py3-none-any.whl (19 kB)
Requirement already satisfied: grpcio<2.0,>=1.24.3 in /home/chenchen/tmp/venv/grpc01/lib/python3.7/site-packages (from tensorflow) (1.44.0)
Collecting tf-estimator-nightly==2.8.0.dev2021122109
  Downloading tf_estimator_nightly-2.8.0.dev2021122109-py2.py3-none-any.whl (462 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 462.5/462.5 KB 6.2 MB/s eta 0:00:00
Collecting keras<2.9,>=2.8.0rc0
  Downloading keras-2.8.0-py2.py3-none-any.whl (1.4 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.4/1.4 MB 870.0 kB/s eta 0:00:00
Collecting keras-preprocessing>=1.1.1
  Downloading Keras_Preprocessing-1.1.2-py2.py3-none-any.whl (42 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 42.6/42.6 KB 7.5 MB/s eta 0:00:00
Collecting tensorboard<2.9,>=2.8
  Downloading tensorboard-2.8.0-py3-none-any.whl (5.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 5.8/5.8 MB 488.6 kB/s eta 0:00:00
Requirement already satisfied: setuptools in /home/chenchen/tmp/venv/grpc01/lib/python3.7/site-packages (from tensorflow) (47.1.0)
Requirement already satisfied: six>=1.12.0 in /home/chenchen/tmp/venv/grpc01/lib/python3.7/site-packages (from tensorflow) (1.16.0)
Collecting astunparse>=1.6.0
  Downloading astunparse-1.6.3-py2.py3-none-any.whl (12 kB)
Collecting h5py>=2.9.0
  Downloading h5py-3.6.0-cp37-cp37m-manylinux_2_12_x86_64.manylinux2010_x86_64.whl (4.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.1/4.1 MB 1.1 MB/s eta 0:00:00
Collecting wheel<1.0,>=0.23.0
  Downloading wheel-0.37.1-py2.py3-none-any.whl (35 kB)
Collecting cached-property
  Downloading cached_property-1.5.2-py2.py3-none-any.whl (7.6 kB)
Collecting markdown>=2.6.8
  Downloading Markdown-3.3.6-py3-none-any.whl (97 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 97.8/97.8 KB 18.7 MB/s eta 0:00:00
Collecting tensorboard-plugin-wit>=1.6.0
  Downloading tensorboard_plugin_wit-1.8.1-py3-none-any.whl (781 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 781.3/781.3 KB 3.4 MB/s eta 0:00:00
Collecting requests<3,>=2.21.0
  Downloading requests-2.27.1-py2.py3-none-any.whl (63 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 63.1/63.1 KB 6.5 MB/s eta 0:00:00
Collecting tensorboard-data-server<0.7.0,>=0.6.0
  Downloading tensorboard_data_server-0.6.1-py3-none-manylinux2010_x86_64.whl (4.9 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.9/4.9 MB 422.5 kB/s eta 0:00:00
Collecting werkzeug>=0.11.15
  Downloading Werkzeug-2.0.3-py3-none-any.whl (289 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 289.2/289.2 KB 227.6 kB/s eta 0:00:00
Collecting google-auth<3,>=1.6.3
  Downloading google_auth-2.6.0-py2.py3-none-any.whl (156 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 156.3/156.3 KB 11.0 MB/s eta 0:00:00
Collecting google-auth-oauthlib<0.5,>=0.4.1
  Downloading google_auth_oauthlib-0.4.6-py2.py3-none-any.whl (18 kB)
Collecting rsa<5,>=3.1.4
  Downloading rsa-4.8-py3-none-any.whl (39 kB)
Collecting cachetools<6.0,>=2.0.0
  Downloading cachetools-5.0.0-py3-none-any.whl (9.1 kB)
Collecting pyasn1-modules>=0.2.1
  Downloading pyasn1_modules-0.2.8-py2.py3-none-any.whl (155 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 155.3/155.3 KB 12.0 MB/s eta 0:00:00
Collecting requests-oauthlib>=0.7.0
  Downloading requests_oauthlib-1.3.1-py2.py3-none-any.whl (23 kB)
Collecting importlib-metadata>=4.4
  Downloading importlib_metadata-4.11.2-py3-none-any.whl (17 kB)
Collecting idna<4,>=2.5
  Downloading idna-3.3-py3-none-any.whl (61 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 61.2/61.2 KB 24.7 MB/s eta 0:00:00
Collecting charset-normalizer~=2.0.0
  Downloading charset_normalizer-2.0.12-py3-none-any.whl (39 kB)
Collecting certifi>=2017.4.17
  Downloading certifi-2021.10.8-py2.py3-none-any.whl (149 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 149.2/149.2 KB 11.5 MB/s eta 0:00:00
Collecting urllib3<1.27,>=1.21.1
  Downloading urllib3-1.26.8-py2.py3-none-any.whl (138 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 138.7/138.7 KB 17.9 MB/s eta 0:00:00
Collecting zipp>=0.5
  Downloading zipp-3.7.0-py3-none-any.whl (5.3 kB)
Collecting pyasn1<0.5.0,>=0.4.6
  Downloading pyasn1-0.4.8-py2.py3-none-any.whl (77 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 77.1/77.1 KB 24.6 MB/s eta 0:00:00
Collecting oauthlib>=3.0.0
  Downloading oauthlib-3.2.0-py3-none-any.whl (151 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 151.5/151.5 KB 13.2 MB/s eta 0:00:00
Using legacy 'setup.py install' for termcolor, since package 'wheel' is not installed.
Installing collected packages: tf-estimator-nightly, termcolor, tensorboard-plugin-wit, pyasn1, libclang, keras, flatbuffers, certifi, cached-property, zipp, wrapt, wheel, werkzeug, urllib3, typing-extensions, tensorflow-io-gcs-filesystem, tensorboard-data-server, rsa, pyasn1-modules, oauthlib, numpy, idna, google-pasta, gast, charset-normalizer, cachetools, absl-py, requests, opt-einsum, keras-preprocessing, importlib-metadata, h5py, google-auth, astunparse, requests-oauthlib, markdown, google-auth-oauthlib, tensorboard, tensorflow
  Running setup.py install for termcolor ... done
Successfully installed absl-py-1.0.0 astunparse-1.6.3 cached-property-1.5.2 cachetools-5.0.0 certifi-2021.10.8 charset-normalizer-2.0.12 flatbuffers-2.0 gast-0.5.3 google-auth-2.6.0 google-auth-oauthlib-0.4.6 google-pasta-0.2.0 h5py-3.6.0 idna-3.3 importlib-metadata-4.11.2 keras-2.8.0 keras-preprocessing-1.1.2 libclang-13.0.0 markdown-3.3.6 numpy-1.21.5 oauthlib-3.2.0 opt-einsum-3.3.0 pyasn1-0.4.8 pyasn1-modules-0.2.8 requests-2.27.1 requests-oauthlib-1.3.1 rsa-4.8 tensorboard-2.8.0 tensorboard-data-server-0.6.1 tensorboard-plugin-wit-1.8.1 tensorflow-2.8.0 tensorflow-io-gcs-filesystem-0.24.0 termcolor-1.1.0 tf-estimator-nightly-2.8.0.dev2021122109 typing-extensions-4.1.1 urllib3-1.26.8 werkzeug-2.0.3 wheel-0.37.1 wrapt-1.14.0 zipp-3.7.0
(grpc01) [chenchen@localhost bin]$ 
```

```shell
(grpc01) [chenchen@grpc01 grpc01]$ ./bin/python3 -m pip install tensorflow
Collecting tensorflow
  Downloading tensorflow-2.8.0-cp37-cp37m-manylinux2010_x86_64.whl (497.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 497.5/497.5 MB 2.9 MB/s eta 0:00:00
Collecting libclang>=9.0.1
  Downloading libclang-13.0.0-py2.py3-none-manylinux1_x86_64.whl (14.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 14.5/14.5 MB 455.4 kB/s eta 0:00:00
Collecting keras<2.9,>=2.8.0rc0
  Downloading keras-2.8.0-py2.py3-none-any.whl (1.4 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.4/1.4 MB 17.7 MB/s eta 0:00:00
Collecting h5py>=2.9.0
  Downloading h5py-3.6.0-cp37-cp37m-manylinux_2_12_x86_64.manylinux2010_x86_64.whl (4.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.1/4.1 MB 190.9 kB/s eta 0:00:00
Collecting tensorflow-io-gcs-filesystem>=0.23.1
  Downloading tensorflow_io_gcs_filesystem-0.24.0-cp37-cp37m-manylinux_2_12_x86_64.manylinux2010_x86_64.whl (2.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.1/2.1 MB 85.5 kB/s eta 0:00:00
Collecting google-pasta>=0.1.1
  Downloading google_pasta-0.2.0-py3-none-any.whl (57 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 57.5/57.5 KB 9.9 MB/s eta 0:00:00
Collecting absl-py>=0.4.0
  Downloading absl_py-1.0.0-py3-none-any.whl (126 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 126.7/126.7 KB 17.0 MB/s eta 0:00:00
Collecting numpy>=1.20
  Downloading numpy-1.21.5-cp37-cp37m-manylinux_2_12_x86_64.manylinux2010_x86_64.whl (15.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 15.7/15.7 MB 16.1 MB/s eta 0:00:00
Collecting opt-einsum>=2.3.2
  Downloading opt_einsum-3.3.0-py3-none-any.whl (65 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 65.5/65.5 KB 15.0 MB/s eta 0:00:00
Requirement already satisfied: setuptools in ./lib/python3.7/site-packages (from tensorflow) (60.9.3)
Collecting keras-preprocessing>=1.1.1
  Downloading Keras_Preprocessing-1.1.2-py2.py3-none-any.whl (42 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 42.6/42.6 KB 12.4 MB/s eta 0:00:00
Collecting typing-extensions>=3.6.6
  Downloading typing_extensions-4.1.1-py3-none-any.whl (26 kB)
Collecting tensorboard<2.9,>=2.8
  Downloading tensorboard-2.8.0-py3-none-any.whl (5.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 5.8/5.8 MB 17.2 MB/s eta 0:00:00
Collecting gast>=0.2.1
  Downloading gast-0.5.3-py3-none-any.whl (19 kB)
Collecting termcolor>=1.1.0
  Downloading termcolor-1.1.0.tar.gz (3.9 kB)
  Preparing metadata (setup.py) ... done
Collecting flatbuffers>=1.12
  Downloading flatbuffers-2.0-py2.py3-none-any.whl (26 kB)
Requirement already satisfied: six>=1.12.0 in ./lib/python3.7/site-packages (from tensorflow) (1.16.0)
Requirement already satisfied: grpcio<2.0,>=1.24.3 in ./lib/python3.7/site-packages (from tensorflow) (1.44.0)
Requirement already satisfied: protobuf>=3.9.2 in ./lib/python3.7/site-packages (from tensorflow) (3.19.4)
Collecting tf-estimator-nightly==2.8.0.dev2021122109
  Downloading tf_estimator_nightly-2.8.0.dev2021122109-py2.py3-none-any.whl (462 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 462.5/462.5 KB 15.2 MB/s eta 0:00:00
Collecting wrapt>=1.11.0
  Downloading wrapt-1.14.0-cp37-cp37m-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl (75 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 75.2/75.2 KB 17.3 MB/s eta 0:00:00
Collecting astunparse>=1.6.0
  Downloading astunparse-1.6.3-py2.py3-none-any.whl (12 kB)
Collecting wheel<1.0,>=0.23.0
  Downloading wheel-0.37.1-py2.py3-none-any.whl (35 kB)
Collecting cached-property
  Downloading cached_property-1.5.2-py2.py3-none-any.whl (7.6 kB)
Collecting google-auth-oauthlib<0.5,>=0.4.1
  Downloading google_auth_oauthlib-0.4.6-py2.py3-none-any.whl (18 kB)
Collecting markdown>=2.6.8
  Downloading Markdown-3.3.6-py3-none-any.whl (97 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 97.8/97.8 KB 19.9 MB/s eta 0:00:00
Collecting tensorboard-plugin-wit>=1.6.0
  Downloading tensorboard_plugin_wit-1.8.1-py3-none-any.whl (781 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 781.3/781.3 KB 2.2 MB/s eta 0:00:00
Collecting tensorboard-data-server<0.7.0,>=0.6.0
  Downloading tensorboard_data_server-0.6.1-py3-none-manylinux2010_x86_64.whl (4.9 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.9/4.9 MB 60.5 kB/s eta 0:00:00
Collecting google-auth<3,>=1.6.3
  Downloading google_auth-2.6.0-py2.py3-none-any.whl (156 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 156.3/156.3 KB 53.4 kB/s eta 0:00:00
Collecting werkzeug>=0.11.15
  Downloading Werkzeug-2.0.3-py3-none-any.whl (289 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 289.2/289.2 KB 55.0 kB/s eta 0:00:00
Collecting requests<3,>=2.21.0
  Downloading requests-2.27.1-py2.py3-none-any.whl (63 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 63.1/63.1 KB 59.5 kB/s eta 0:00:00
Collecting rsa<5,>=3.1.4
  Downloading rsa-4.8-py3-none-any.whl (39 kB)
Collecting cachetools<6.0,>=2.0.0
  Downloading cachetools-5.0.0-py3-none-any.whl (9.1 kB)
Collecting pyasn1-modules>=0.2.1
  Downloading pyasn1_modules-0.2.8-py2.py3-none-any.whl (155 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 155.3/155.3 KB 55.6 kB/s eta 0:00:00
Collecting requests-oauthlib>=0.7.0
  Downloading requests_oauthlib-1.3.1-py2.py3-none-any.whl (23 kB)
Collecting importlib-metadata>=4.4
  Downloading importlib_metadata-4.11.2-py3-none-any.whl (17 kB)
Collecting urllib3<1.27,>=1.21.1
  Downloading urllib3-1.26.8-py2.py3-none-any.whl (138 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 138.7/138.7 KB 58.9 kB/s eta 0:00:00
Collecting certifi>=2017.4.17
  Downloading certifi-2021.10.8-py2.py3-none-any.whl (149 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 149.2/149.2 KB 72.8 kB/s eta 0:00:00
Collecting idna<4,>=2.5
  Downloading idna-3.3-py3-none-any.whl (61 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 61.2/61.2 KB 62.5 kB/s eta 0:00:00
Collecting charset-normalizer~=2.0.0
  Downloading charset_normalizer-2.0.12-py3-none-any.whl (39 kB)
Collecting zipp>=0.5
  Downloading zipp-3.7.0-py3-none-any.whl (5.3 kB)
Collecting pyasn1<0.5.0,>=0.4.6
  Downloading pyasn1-0.4.8-py2.py3-none-any.whl (77 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 77.1/77.1 KB 50.5 kB/s eta 0:00:00
Collecting oauthlib>=3.0.0
  Downloading oauthlib-3.2.0-py3-none-any.whl (151 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 151.5/151.5 KB 56.0 kB/s eta 0:00:00
Using legacy 'setup.py install' for termcolor, since package 'wheel' is not installed.
Installing collected packages: tf-estimator-nightly, termcolor, tensorboard-plugin-wit, pyasn1, libclang, keras, flatbuffers, certifi, cached-property, zipp, wrapt, wheel, werkzeug, urllib3, typing-extensions, tensorflow-io-gcs-filesystem, tensorboard-data-server, rsa, pyasn1-modules, oauthlib, numpy, idna, google-pasta, gast, charset-normalizer, cachetools, absl-py, requests, opt-einsum, keras-preprocessing, importlib-metadata, h5py, google-auth, astunparse, requests-oauthlib, markdown, google-auth-oauthlib, tensorboard, tensorflow
  Running setup.py install for termcolor ... done
Successfully installed absl-py-1.0.0 astunparse-1.6.3 cached-property-1.5.2 cachetools-5.0.0 certifi-2021.10.8 charset-normalizer-2.0.12 flatbuffers-2.0 gast-0.5.3 google-auth-2.6.0 google-auth-oauthlib-0.4.6 google-pasta-0.2.0 h5py-3.6.0 idna-3.3 importlib-metadata-4.11.2 keras-2.8.0 keras-preprocessing-1.1.2 libclang-13.0.0 markdown-3.3.6 numpy-1.21.5 oauthlib-3.2.0 opt-einsum-3.3.0 pyasn1-0.4.8 pyasn1-modules-0.2.8 requests-2.27.1 requests-oauthlib-1.3.1 rsa-4.8 tensorboard-2.8.0 tensorboard-data-server-0.6.1 tensorboard-plugin-wit-1.8.1 tensorflow-2.8.0 tensorflow-io-gcs-filesystem-0.24.0 termcolor-1.1.0 tf-estimator-nightly-2.8.0.dev2021122109 typing-extensions-4.1.1 urllib3-1.26.8 werkzeug-2.0.3 wheel-0.37.1 wrapt-1.14.0 zipp-3.7.0
(grpc01) [chenchen@grpc01 grpc01]$ 
(grpc01) [chenchen@grpc01 grpc01]$ ./bin/python3 -m pip list
Package                      Version
---------------------------- -------------------
absl-py                      1.0.0
astunparse                   1.6.3
cached-property              1.5.2
cachetools                   5.0.0
certifi                      2021.10.8
charset-normalizer           2.0.12
flatbuffers                  2.0
gast                         0.5.3
google-auth                  2.6.0
google-auth-oauthlib         0.4.6
google-pasta                 0.2.0
grpcio                       1.44.0
grpcio-tools                 1.44.0
h5py                         3.6.0
idna                         3.3
importlib-metadata           4.11.2
keras                        2.8.0
Keras-Preprocessing          1.1.2
libclang                     13.0.0
Markdown                     3.3.6
numpy                        1.21.5
oauthlib                     3.2.0
opt-einsum                   3.3.0
pip                          22.0.4
protobuf                     3.19.4
pyasn1                       0.4.8
pyasn1-modules               0.2.8
requests                     2.27.1
requests-oauthlib            1.3.1
rsa                          4.8
setuptools                   60.9.3
six                          1.16.0
tensorboard                  2.8.0
tensorboard-data-server      0.6.1
tensorboard-plugin-wit       1.8.1
tensorflow                   2.8.0
tensorflow-io-gcs-filesystem 0.24.0
termcolor                    1.1.0
tf-estimator-nightly         2.8.0.dev2021122109
typing_extensions            4.1.1
urllib3                      1.26.8
Werkzeug                     2.0.3
wheel                        0.37.1
wrapt                        1.14.0
zipp                         3.7.0
(grpc01) [chenchen@grpc01 grpc01]$ 
```



## See Also

https://www.tensorflow.org/install