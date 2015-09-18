# Codebook

### Abbreviations and Acronyms
* PSA: prostate-specific antigen
* PCa: prostate cancer
* BX: biopsy
* RC: reclassification
* RRP: surgical removal of prostate (radical retropubic prostatectomy)
* PT: patient
* DX: diagnosis

### PT data
1 record per patient
* id: pt-specific unique identifier, assigned for data generation
* age.dx: age at PCa diagnosis
* sec.time.dx: “secular time”, calendar date of diagnosis. 0 is January 1, 2005.
* eta.true: latent state, either 0 or 1, corresponding to indolent (Gleason <=6) or aggressive (Gleason >=7) cancer (respectively); generated for all patients for data simulation but only observed on a subset with surgery (rap)
* obs.eta: eta.true is rrp=1, NA otherwise
* rrp: does this subject ever have surgery?
* rc: does this subject ever reclassify, i.e. have a biopsy with a Gleason score 7 or higher
* std.vol: prostate volume, mean and sd standardized
* subj: pt-specific unique identifier, assigned after data simulation to correspond to data sorting. data sorted based on obs.eta as follows: eta observed and =0, eta observed and =1, eta unobserved. (sorting by obs.eta makes posterior sampling in JAGS much easier)

### PSA data
1 record per person per PSA observation
* psa: outcome measured in continuous time. Measurements may occur before initial PCa diagnosis. 
* log.psa: log-transformed PSA used for regression
* age: age at PSA measurement
* age.std: mean and std dev-standardized age at psa (standardized within this dataset); used in X
* std.vol: prostate volume, standardized within psa dataset; used in Z


### BX Data
1 record per annual interval where pt eligible for biopsy or (after rc) surgery.
Patients eligible for BX until RC (as per active surveillance protocol); it is only possible to reclassify once.
Patients eligible for surgery prior to RC or up to 2 years after RC
Patients censored after surgery, 2 years post-RC, or 10 years
No record for initial diagnostic bx 


* eta: true state (for all patients, used to generate data)
* bx.here: biopsy occurs in this interval; NA after a pt reclassifies
* rc: grade reclassification occurs at this annual biopsy
* rrp: surgery performed in this annual interval

* time, time.ns: years since dx for bx, natural spline representation of years since dx
* age, age.std, age.ns: age at bx, standardized, and ns representation
* sec.time, sec.time.std: calendar time, standardized (see above)
* num.prev.bx: number of prior bx at beginning of interval (used to predict P(BX) in this interval); everyone has one prior bx (diagnostic) at time=1 (
* num.prev.bx.rrp: number of prior bx + bx in this interval (used to predict P(RRP) in this interval)
* prev.G7: bx grade of Gleason 7 or higher (i.e. reclassification) in this interval or previous intervals (used to predict P(RRP) in this interval)

* rm: indicator for removing a record from dataset (used in data generation); not used in analysis

