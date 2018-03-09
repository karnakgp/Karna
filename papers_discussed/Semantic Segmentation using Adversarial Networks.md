### Title

#### Semantic Segmentation using Adversarial Networks

### tl;dr

The segmentation of a image is the task of classifying each pixel to a certain class. Here this is done using an adversarial appraoch rather than the conventional CNN or FCN which were based on single loss term.

### Describe the method

The Network model is as follows :
![Network Model](https://lh3.googleusercontent.com/W5PzxtY72Y_RBhsewDgtLV4nS8Gid-RpnTvN1W3bGOj-nkcYx30kR2i7eHzBO7d22GNljGHS9eWYxHM6zJlvITvMXovMyRw9mwskFMUxcjua9gvEzakintDOGVPCyG7pPTKhnNvQZl_JJOyLQAakyMB3FCqf9N0TZMzGoMAAap6r-dyjUPfR9t2Ox3_sxKvONPB-Ummn537ZAmbJcKBKVosL4auFK-GRA_JwCZD1I8sPyARYukvNFnOo_SvsVkWOF1KDS8tLUJCH9BlAr3-9AqzoHifygarj2meHJPh6whlmY9z41FQyyGzlCOpZs9hQJIQaXtZDSQTLX_Qr1MKc1q3Qg7ZGjA4MdHMSx3ICcjtBbglp2G2yA8c6rtSSNqGnuOM5jv6A5nApWGzD-mnzKawk6KUC9N3f0VeVg_Thh6K1H6gSqIz-VCVBDQZ0_MaCjtRxkpuJA7dj1IlIO8zR9Z4An6WGOanDo2p59XTBuLiQwvj2k07VAolxoBuX6-ubTai0wYj0LgBHvMU2IITBbR5H_Cpq0W2mmKxOS1d-Aij4sjjRS-_73EcisGy_h3jWePqPonKuzJbjxGCKaYbI9X5pmac8JHO1Kpc4uzo=w673-h319-no "Network")

**The two networks are :**
* Segmentor
* Discriminator

**The segmentor** is the traditional ConvNet we have seen that takes as input an image of dimensions HxWxC, here:
H:Height ; W:Width ; C:Channels of the imput image.
And outputs the classpredictions for each pixel of the image. So the output is of shape HxWxS where S is the desired no of classes we want to segment the image to.
The third dimension i.e. the 'S' one contains the probability of a certain pixel belonging to each of those S classes.

**Segmentor's Loss Term** : Multiclass Cross Entropy :
![alt text](https://lh3.googleusercontent.com/PZ8KPmFVZB_fykjTsbauDlN0dY19hDZ21EiYlg_8r_nzHA4il4OvZxU3XbXgJAulyPx92bvLjopHZ4QZikRAAeEOsvTeg9YGPBE5CgPIJ1x8qKoElA0AnArDNRYa5hEYe2ssat3C6CoCJl636V87iXgTb1UlGIJUsUwVMjTqQEFS2xgZZaK-eSJY-sRLfg7fkWPSTAsj3kyODcZyTVIkO2JJNXRwooRROyiwXAsUOp8D-FO_sgbskE87zeZubrAMBAq4Wmo1eUXI2i6b8b_t9ISBz6NhfKlsHoowqOQIENa_q7t03n0Y8sxD1SEgZKiFUpVde-E-n3xMelG0ikds3vYibo_tj95GvhgJ0ALT_T6GRoaGtOYxlmM3TSvPmbtb5mW9keNuQ8TVJnvgOyFJOsbhTnLFrTjlaQfNLOYAG-c73NWay-2VFGaPrYkFwkgwVBuQbKgp1XQtnudCKizlYVZ3HtNKu1819H1li1bEGqxqkUndk58k1WOBqs1udkZiCZstgAeH5osziPyrxVS2IUXLh7Tda13SVyusK3Bu1Bu6OoIFj5mUUCPdPOoCLfJrcOn7JHlC3UHA5au5T3gw1DQioxavdxlLNkEi6sA=w394-h33-no "Segmentor Loss Function")


**The Discriminator** takes as input a label map, and the corresponding RGB image. The label map is either the ground truth corresponding to the image or that predicted by the Segmentor Network.
Initially two separate branches process the label map and the input RGB image. These are converted to 64 channels and finally concatenated . This concatenated signal is then passed through another stack of convolutional and max-pooling layers, after which the binary class probability is produced by a sigmoid activation function. This tells us whether the label map was genuine (coming from ground truth)  or counterfiet (produced by segmentor).

**Adversary's Loss Term** : Binary Cross Entropy
![Adversary Loss Term](https://lh3.googleusercontent.com/t-JFT9gKw-s_zOCvF3ZgYPhiPYp8X8hl4ANvijI3p1iPU76q7JSErA4oMeLFBztCEJQm1LpmkXXr-vIgqjzkcJNPAEQMd4TpHOgKQaI_nXBhMTwSMEIsjdP_r-zVLdrf1CS6j7-Swuw8qZgaOlM1iQZHhJYdeVrZZw37ZQaWC-_ITe2g5TwpNpb4kijysXaaxURzBVJsSvYpzupwknjy9DzeVCLU1WYH_1msHNSYrxC51-4QYU10h5by2gCDoZtEB4TMnu0n0GLzk2m1xvVRLLB79CUAsh1SFvsdPeHO42F6bWhGSDMsoT0DfL61vK7yEtbToj-ykKFYuJCn2LfoxUDK0m17HRVaMfNLsdGlWlSJgftmXtypenRBLdN5AjBbj5c5WG9VrYYeZIkveXNz9IqMnw2hUWsYtPFPFWCbOzTgIz9WngGr2dDCMPxP5VSPytRkbPT1iDCXKOSMVN0bz7SC7rJUeyktihzlJHtFhAZnj4Jd7Pg5RxOTU_fgduZdQv-2AsF30mH8ofg8sKB6y4dh78a7UngmSGpI7EatrL8jjzieUBFe-WJ27jQAgmFRH83CerdWbe3biVhbebNNL4tQPw4fTsq3JKNCt14=w412-h34-no "Adversa Loss Term")


**Overall Loss Term** : Segmentor-*lambda*(Adversarial)

![Model Loss Function](https://lh3.googleusercontent.com/Yj73CzsMM-ZpbWtcUtjEic-9RRZnW-Gs4F0CSukTGLpR9utF1gsiGxVnj1uCIjItScWiTODUEluRK09QxNKEYTgFHmtCGPrW0BkxVljh4QQuEC3gQdx46bozQIxNVkwrUi7cDozA2Xzuu5pbqsKxO0yCtfrwl_lWAI1SWmZGTA4WCkMu_LtGc0lMIKCnqMmCImpw_kyL8yKRqFHJOhiIuhZ9YN9NcOfqRGntyPXyQGbNi277ygReotYGAIcCcOpT8Eihark3takbNhlJnz5Q4SG4AO-32-gjpBanAHb2vASvJzlMrwisL8iz15bWpxSd_WDzP2CMeVYYHYQUvUoahuUM-tJnGKiKfZlX1h6SdTR2YjH1P0kyX3UlU-BrN27vAoiX2Q-vWFkO3L0Yx2LR2szQKYiqIrF3dCI4UbMUbx7JW27AShHIoT54oMHwPCKAuOqEeyU6xhrEU8WUDbuudEj8v0PpJFcmV16vdW_el-VZm1QE-S8iG4098QuHSdYN69-jHSc01NAeZQyHfvltHGRjI6lwjWdePTZTpsbGtrXJOHcu4RfBXuZFCC61LqkaGRwItYOazTyD70DfMH8p7bU1MqR8Hywh-LhwkRU=w830-h91-no "Model Loss Function")

* **N**: number of training examples
* **s(xn)** : output of segmentor
* **yn** : ground truth label
* **θs**: parameters of the segmentation model
* **θa**: parameters of adversarial model

#### Training the model
In GANS we train the two networks one by one at each step.
Here training the Adversarial is minimizing the BCE Loss : ![Adversarial Training](https://lh3.googleusercontent.com/lUqe6ey5ckf-lqNEfrVnqnfPUclYGPIyhLGALOpOLo3PpX_F69D3WxE7rXjZBQ3E7CoRUc4x8LA7sRJNCE0eTBoDkyQKOx1xrxbGbN7-2a94eRGaTxX3AMtlpXU8ArUWW8n0m-6TLduuV8Ty8PMJdW5zHEmimviwcl2I6l7ag3F-f14gmRsqeLEMa7DFC6QGqzReKIxGru0PCC9RkOQ-NUVveLp2WTFdQDqfhdldFrGlCQ8UvBuwE_wfRZvn16ohlvnwHdt56koE8TFFX1EGxKNa_qBVFFHrBpjfqH7ajehKPPQgQQaCg7ggmMI_rRzQ2acX_QQTnbj_IjzVuskvdDjGn5eZedItPsl2gNMlU6nneZQIwvcAWyq6zsnbOnOhwth_clAoHSWhLo6h0wqpUArZCX0qsIQ4VeUcIvS3JK3Y1LO9bxsbe0A-hbwHpzCSxCCF66hZmXxUurQw4hSAh03R7Heb_jpYNdAySe9I7nrReSJMEheIEhLD3jsuncmHp4Hc4O_oKKpRxeigrESO7KUYbQfuuMziidohwjiyJnJpHBlSISmrU3-EMcge7USaY4SIEquwv_867Yh4Gmn6Y-ylLd3XElltO8Ocy7E=w480-h84-no "adversarial training")
And training the Segmentor is equivalent to minimizing the multi-class cross-entropy loss, while at the same time reducing the performance of the adversarial network. This encourages the segmentation model to produce segmentation maps that are hard to distinguish from ground-truth ones for the adversarial model. This is the classic mini max game of Adversarial networks.
ie :
![segmentor training](https://lh3.googleusercontent.com/SHW26pgtyKOc1dwPTkQRN0hLg8-up8DNNylUyHvo6QJYMpEfDr_fAKOpENmrL-r6_jU_1zdTI1JdJerk5R8vx0PDRt44WUPiT30-hcuMWDp-S2qcOM09EVwS8fOFs1eTUK320yK96izx7sNmMgM2EY-1TrxrVlO4nniKapoy3Wzb0OKkgN8FVIbJxD0t-S-GI-At2bm2tCFUsHfd888TDOnzBbLZ55xwy6IHQ8HBS1KW3qIcMSqgEAGCMDVnDbxUxe6I_KMjEC8NSifDohIquVLzJbcf5q6Ud03OvxLv4Ul8NGfw2_1QbwW1T8mWxC0hrjP-mfSOKxrOFQuQn-EDXHdvE4w00RlD8geN_zBnAlW4CNTpKcqDSrqIJVXOZW1xtAP5nYKOazxDqEutBBxG17fsZx3WoEuVn3jPGlJMrzo59sM8L4DNqtcBqrW8ogew0lfTWU9bmw36iZhr8AKJWGE3hNxs0Cj4CBk4nvWOG4EeS3HP5p7rbaSNoNY6D6M8poPqtyRqK5QDsZbXi7AaLIjzTTQLqFbLpQ-9GEhN_Ki89oNAkPddht3rCnsCqyyw7j3hFdsy-zxKbi3_Pe1S6GCdEygeBHJnIs5StjY=w473-h94-no "segmentor training")

### Any further details

In the Segmentor's training funtion, we replace the second term by ![alt](https://lh3.googleusercontent.com/bVPQWqF4_mDWe_NpmokrgBamG6jLe2g6RgKQhJc8MxaswQxDr8r4O4FLbPPqlX7VGHtiSkpbgusiN2ehUaAqqzJiF-zAyV7xixkhNKKlNh0B5kKiqEPEmpNqyvfq9IPoi_AdnehAhkf52WXG_DpCL7QVyEMh8mUIV4APksilcKAdvAdqpqJ4u9GpFXTXjU8K4LSgWLoK9gy8KoKpBpTXqPDRu7N_fudzO55JMI_6rwX4dV2PVAMHXgT-jQtJS_Vfq--oEAVURSd13bGxH41Y9rnQAP4k6T30UxPdkmckE8TLILpc5KFJXJyPdfvS53D_hq2W08Xj8uWCnD_V6hLS9dQqK2rgvx9d90GjlB93Y43nVDnEAcOUmx9AHynZM9RalnDe-mbC4EURBLdGVJOrdu0h3c8aWwK5_iReOjkmOb3_8gOCde5mE8TY4dTuPvEEHmyyqzbTuGJ56TVcTex7R1ubXW8HD3Ez-XXcnsY3PtbMCOzjXmKGRXUKhtomQ_3hznmlNwvdKcNaLKsJFiISGwbfGM1WpDo9gHushrueesaVmAIviHeI-KD4BRWCb5RJPAZDwu2J41Rc5lTA8MfTDD10CZ5DhJTdf5_pB_w=w250-h34-no "seg model")
The reason behind this is described in https://arxiv.org/pdf/1406.2661.pdf .


### My two cents

Although other segmentation models exist but their perception of the image is not global, here due to the presence of the adversary which rejects the entire image's segmentation as original or fake we can learn the globally and not just on the pixel scale.
