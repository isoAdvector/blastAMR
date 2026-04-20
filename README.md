# A Load-balanced adaptive mesh refinement for OpenFOAM

> [!IMPORTANT]
> **This is a fork of [STFS-TUDa/blastAMR](https://github.com/STFS-TUDa/blastAMR).**
> It was originally created to fix a bug in the `hexRefiner` unrefinement/cleanup logic.
> The scope has since expanded to update the codebase for compatibility with
> OpenFOAM-v2512 and newer OpenCFD releases.
> For the upstream project, original documentation, and full contributor list, see the
> [original repository](https://github.com/STFS-TUDa/blastAMR).

<details open><summary>H2 injection case</summary>

https://github.com/STFS-TUDa/blastAMR/assets/34474472/44cdc485-0c90-41b0-90b7-956e013abf9c

![2023-10-10_14-30](https://github.com/STFS-TUDa/blastAMR/assets/34474472/086dec0e-392f-4cf5-a769-d91f174a7b33)

</details>

<details open><summary>Free-propagating flame case</summary>

https://github.com/STFS-TUDa/blastAMR/assets/34474472/29d1cf64-ade9-4c5c-8044-8e63797d31b7

![2023-10-10_14-29](https://github.com/STFS-TUDa/blastAMR/assets/34474472/cab32728-b497-438b-8150-940a3449a188)

</details>

This repository extracts library parts from [blastFoam](https://github.com/synthetik-technologies/blastfoam) which are relevant to load-balanced adaptive mesh refinement for polyhedral meshes while retaining the original commit history; this is not intended as a full port! Only **tested features** are to be trusted. **!!WIP!!**

To get started, you can visit the [upstream wiki](https://github.com/STFS-TUDa/blastAMR/wiki) — note that it documents the original repository and may not reflect changes introduced in this fork.

## Objectives

- Have a reasonable Load-balanced AMR for 2D/3D OpenFOAM meshes in the **OpenCFD versions**.
- Support load balancing of Lagrangian particle clouds (not yet implemented).
- Make it easy to include this lib in other projects through submodules/subtrees.

`blastFoam` is GPL licensed; see the included (original) [COPYING](COPYING).

## Quick Notes

- Everything compiles to your `$FOAM_USER_LIBBIN` with a recent OpenCFD ([openfoam.com](https://openfoam.com)) version.
- Our favorite refinement cell selector is the [`coded`](https://github.com/STFS-TUDa/blastAMR/blob/0e70469d718ee53a5ce892e0e24a4f940bfba369/tutorials/poly_freelyPropFlame2D_H2/constant/dynamicMeshDict#L25). Although a wide range of selectors is provided, they are not well tested. So, use the coded refinement whenever possible.
- The polyRefiner currently does not incorporate a history restraint to keep subcell siblings on same processor, which means that unrefinement/cleanup is not done properly and the mesh may be littered with fine cells. If your mesh is a hex mesh, use the `hexRefiner` instead — the unrefinement bug in `hexRefiner` has been fixed in this fork.
