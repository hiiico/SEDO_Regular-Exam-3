/*
 * My Jenkinsfile from Repo 3 defines an automated pipeline for a .NET project on Linux.
 * Key features:
 * - Automatically triggers on SCM changes (polling every minute)
 * - Verifies .NET SDK availability and installs it if missing (v8.0.100)
 * - Restores dependencies, builds, and runs UI tests
 * 
 * Pipeline flow:
 * 1. Checks out source code from SCM
 * 2. Verifies if .NET SDK is installed
 * 3. Conditionally installs .NET SDK if not found
 * 4. Restores NuGet dependencies
 * 5. Builds the project
 * 6. Executes UI tests
 */
